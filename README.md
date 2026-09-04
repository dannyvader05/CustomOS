# CustomOS

A custom x86 operating system built from scratch — bootloader, kernel, and system-level infrastructure implemented in raw x86 Assembly and C, with no reliance on an existing OS or standard library.

**Status:** 🚧 In active development — currently at the bootloader stage (real-mode boot sector, BIOS-based screen output). Kernel, interrupt handling, and filesystem support are in progress.

## Overview

This project implements an operating system from the ground up, covering the full stack from the boot sector through kernel-space memory management, interrupt handling, and (eventually) userland process execution. The goal is to build a functional, minimal OS while understanding and implementing every layer manually — no existing kernel, bootloader, or libc is reused.

## Toolchain

- **Assembler:** NASM
- **Compiler:** custom-built `i686-elf` cross-compiler (GCC + Binutils), targeting freestanding bare-metal x86
- **Emulator:** QEMU
- **Debugger:** GDB, via QEMU's remote debug stub
- **Build environment:** Linux (WSL2 on Windows)

## Progress

### ✅ Completed
- 512-byte MBR bootloader (real mode)
- BIOS interrupt-based screen output

### 🔧 In Progress / Planned

**Bootloader stage**
- VGA text-mode customization (colors, cursor control)
- Boot-time system monitor — memory size and disk geometry detection via BIOS (`e820`), reported before handoff to the kernel

**Kernel stage**
- Disk I/O and transition to a protected-mode C kernel
- Physical memory allocator and a `kmalloc` heap
- Multiboot-compliant memory map parsing

**Interrupts & I/O**
- GDT (Global Descriptor Table) and IDT (Interrupt Descriptor Table) setup
- ISR/IRQ handling
- Keyboard driver and an interactive shell
- PIT (timer) driver
- Stub round-robin task scheduler

**Memory & Filesystem**
- Paging / virtual memory
- FAT filesystem parsing
- ELF binary loading
- A VFS (virtual filesystem) abstraction layer

**Multitasking**
- Real context switching (register + stack state save/restore)
- Process control blocks
- Cooperative-to-preemptive scheduler evolution

**Userland**
- Ring 3 (user-mode) execution
- A syscall interface
- A minimal libc (`write`, `exit`, `malloc`)
- A small set of test applications running as real user-mode processes

**Stretch goals**
- Framebuffer/VBE graphics mode
- Signals / inter-process communication
- Networking (NIC driver + a TCP/IP stack) — to be scoped later depending on remaining time

## Building & Running

```bash
make
qemu-system-i386 -fda build/main_floppy.img
```

## Why this project

Most software today is built several layers above the hardware. This project exists to understand and implement those layers directly — boot process, protected mode, interrupts, memory management, and process execution — rather than treating them as opaque infrastructure.
