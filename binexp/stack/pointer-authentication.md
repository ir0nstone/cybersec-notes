---
description: An Arm hardware protection to combat ROP
---

# Pointer Authentication

## Overview

Pointer Authentication is a hardware feature available for Arm devices to protect against ROP attacks. A **Pointer Authentication Code** (PAC) is generated from the value of a given pointer, and must be used to verify pointers before using them. This protection requires **hardware support**, as the assembly instructions (such as [`paciasp`](https://developer.arm.com/documentation/ddi0602/2024-03/Base-Instructions/PACIA--PACIA1716--PACIASP--PACIAZ--PACIZA--Pointer-Authentication-Code-for-Instruction-address--using-key-A-) and [`retaa`](https://developer.arm.com/documentation/ddi0602/2025-12/Base-Instructions/RETAA--RETAB--Return-from-subroutine--with-pointer-authentication-)) that are required for this must exist on the processor, and **compiler support**.

PAC has two keys, called Key A and Key B. The instruction `paciasp` will sign the Link Register (`lr`) using Key A and the SP register, and is often used at function entry (to store the return pointer). `pacibsp` will do the same, but with Key B.

At function exit, when LR is popped off the stack, we use the `retaa` instruction instead. This instruction authenticates the address in LR using Key A and SP and branches to the authenticated address. `retab` is used for Key B instead.
