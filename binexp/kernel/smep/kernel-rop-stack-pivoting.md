# Kernel ROP - Stack Pivoting

While the kernel cannot execute code in userland, it _can_ set its RSP to a userland location, so it is possible to stack pivot to userland as long as all of the gadgets used are in kernel space.

I don't think an example is necessary for this.
