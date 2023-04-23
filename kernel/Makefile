
all:
	nasm -f bin ../boot/bootloader.asm -o bootloader.bin
	gcc  -ffreestanding -fno-stack-protector  -fno-pie -c kernel.c -o kernel.o -m32
	gcc -ffreestanding -fno-stack-protector  -fno-pie -c ../lib/stdio.c -o stdio.o  -m32
	gcc -ffreestanding -fno-stack-protector  -fno-pie -c ../lib/stdlib.c -o stdlib.o  -m32
	gcc  -ffreestanding -fno-stack-protector  -fno-pie -c ../lib/string.c -o string.o  -m32
	gcc  -ffreestanding -fno-stack-protector  -fno-pie -c ../drivers/video.c -o video.o  -m32
	gcc  -ffreestanding -fno-stack-protector  -fno-pie -c ../drivers/keyboard.c -o keyboard.o  -m32
	nasm  -f elf32 kernel_head.asm -o kernel_head.o 
	ld  -melf_i386 -o kernel.bin -Ttext 0x1000 kernel_head.o kernel.o stdlib.o stdio.o string.o video.o keyboard.o --oformat binary -T link.ld
	cat bootloader.bin kernel.bin > ../pingukernel_230423-1.0.img
	rm *.bin *.o

flash-to-drive:
	dd if=/dev/zero bs=512  count=200 of=/dev/sdb
	dd if=../pingukernel_230423-1.0.img of=/dev/sdb


run: 
	qemu-system-x86_64 ../pingukernel_230423-1.0.img