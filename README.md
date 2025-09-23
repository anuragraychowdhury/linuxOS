### ECE391 Linux‑like Operating System (Educational Kernel)

A minimal Linux‑like operating system built from scratch for the Illinois ECE391 course. It demonstrates core OS concepts—bootstrapping via GRUB, protected‑mode execution, paging, interrupts, system calls, device drivers, a custom file system, and a simple shell—running on x86 (i386) in QEMU.

#### Tech Stack
- C (primary) + x86 Assembly (AT&T)
- x86‑32 (i386), Multiboot v1, GRUB
- i686‑elf GCC/binutils cross‑toolchain
- QEMU for emulation

#### Key Features
- Multi‑terminal support with fast switching  
- Virtual memory: 4KB/4MB paging  
- Custom file system with inodes  
- System call interface (int 0x80)  
- Interrupt‑driven kernel with PIC  
- Keyboard and RTC drivers  
- Basic process management and context switch  
- Comprehensive test suite

#### System Architecture
- Multiboot entry → kernel `entry` in C  
- GDT/TSS setup, IDT‑based interrupts  
- Ring 0 kernel, Ring 3 user space  
- Paging and memory protection  
- File operations via unified FD table  
- Drivers: keyboard, RTC, terminal  
- Simple scheduler and PCB model  
- Static linking; no libc, bare‑metal

#### Project Layout
- `student-distrib/boot.S` — Multiboot, CPU setup, jump to C  
- `student-distrib/kernel.c` — Kernel init, IDT/PIC/drivers/tests  
- `student-distrib/paging.c,h` — Page tables and mappings  
- `student-distrib/IDT.c,h` — Interrupt descriptor table  
- `student-distrib/i8259.c,h` — PIC programming  
- `student-distrib/syscall.c,h` — Syscall dispatch (0x80)  
- `student-distrib/filesystem.c,h` — FS structures/ops  
- `student-distrib/keyboard.c,h`, `rtc.c,h`, `terminal.c,h` — Drivers  
- `student-distrib/tests.c,h` — Validation tests

#### Build and Run (macOS)
1) Install tools:
```bash
brew install i686-elf-gcc i686-elf-binutils qemu
```
2) Build:
```bash
cd student-distrib
make clean && make
```
3) Run in QEMU:
```bash
qemu-system-i386 -drive format=raw,file=mp3.img -drive format=raw,file=filesys_img -m 128M -nographic
```
At GRUB, press Enter (or b). You should see kernel output.

#### Notes
- Uses a cross‑compiler; host `gcc`/`as` will not work.  
- Runs entirely in QEMU; no host OS dependencies at runtime.  
- Educational kernel: minimal, readable, and intentionally simple.
