markdown


Copy code
# Building and Running the OS

## Setup

```bash
export PREFIX="/usr/local/i386elfgcc"
export TARGET=i386-elf
export PATH="$PREFIX/bin:$PATH"
Verify the compiler installation:

bash


Copy code
i386-elf-gcc --version
Building
Compile the kernel:

bash


Copy code
i386-elf-gcc -ffreestanding -c kernel/kernel.c -o kernel.o
Assemble the kernel entry:

bash


Copy code
nasm boot/kernel_entry.asm -f elf -o kernel_entry.o
Link the kernel:

bash


Copy code
i386-elf-ld -o kernel.bin -Ttext 0x1000 kernel_entry.o kernel.o --oformat binary
Assemble the bootloader:

bash


Copy code
nasm boot/bootsect.asm -f bin -o bootsect.bin
Create the final disk image:

bash


Copy code
cat bootsect.bin kernel.bin > os-image.bin
Running
Run the OS in QEMU:

bash


Copy code
qemu-system-x86_64 -fda os-image.bin
Initial Makefile
Alternatively, use the provided Makefile or follow the instructions from Lesson 11 at https://wiki.osdev.org/GCC_Cross-Compiler.

Note: You may need to load the PREFIX and TARGET variables after each system restart.

Installing GDB
To install GDB for debugging, follow these steps:

bash


Copy code
export PREFIX="/usr/local/i386elfgcc"
export TARGET=i386-elf
wget "http://ftp.gnu.org/gnu/gdb/gdb-7.11.tar.gz"
tar -xvzf gdb-7.11.tar.gz
cd gdb-7.11
./configure --target="$TARGET" --prefix="$PREFIX" --program-prefix=i386-elf-
make
make install
Run these commands as root.
