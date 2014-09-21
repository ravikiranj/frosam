frosam
======
PROLOGUE
-----------
* Read introductory material on [OSDev](http://wiki.osdev.org/Main_Page)
* Setup [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler).
  * The below version combination worked for me in Ubuntu 14.04.
    * binutils-2.24.tar.bz2
    * gcc-4.8.2.tar.bz2
    * gmp-5.1.1.tar.bz2
    * mpc-1.0.2.tar.bz2
    * mpfr-3.1.2.tar.bz2
  * Change binutils and gcc configure commands as below while following the twiki.
   ```
   # binutils
   ../binutils-x.y.z/configure --target=$TARGET --program-prefix="$TARGET-" --prefix="$PREFIX" --with-sysroot --disable-nls --disable-werror
   
   # gcc
   ../gcc-x.y.z/configure --target=$TARGET --program-prefix="$TARGET-" --prefix="$PREFIX" --disable-nls --enable-languages=c,c++ --without-headers
   ```

Building the OS
----------------
* Put the below bash function and use it as go to help for building the OS
```
function kc  
{
    echo '
# Compile Kernel
i686-elf-gcc -c kernel.c -o kernel.o -std=gnu99 -ffreestanding -O2 -Wall -Wextra
# Linker
i686-elf-gcc -T linker.ld -o frosam.bin -ffreestanding -O2 -nostdlib boot.o kernel.o terminal.o -lgcc
# Copy bin
cp frosam.bin ../isodir/boot/
# Grub
grub-mkrescue -o ../frosam.iso ../isodir
# Load
qemu-system-i386 -cdrom ../frosam.iso
'
}
```
