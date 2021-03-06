# Makefile for the OS projects.
# Best viewed with tabs set to 4 spaces.

CC = gcc -Wall -Wextra -std=c99 -g
LD = ld

# Where to locate the kernel in memory
KERNEL_ADDR	= 0x1000

# Compiler flags
#-fno-builtin:			Don't recognize builtin functions that do not begin with
#						'__builtin_' as prefix.
#
#-fomit-frame-pointer:	Don't keep the frame pointer in a register for 
#						functions that don't need one.
#
#-make-program-do-what-i-want-it-to-do:
#						Turn on all friendly compiler flags.
#
#-O2:					Turn on all optional optimizations except for loop unrolling
#						and function inlining.
#
#-c:					Compile or assemble the source files, but do not link.
#
#-Wall:					All of the `-W' options combined (all warnings on)

CCOPTS = -c -fomit-frame-pointer -O2 -fno-builtin

# Linker flags
#-nostartfiles:			Do not use the standard system startup files when linking.
#
#-nostdlib:				Don't use the standard system libraries and startup files when
#						linking. Only the files you specify will be passed to the linker.
#          
#-Ttext X:				Use X as the starting address for the text segment of the output 
#						file.

LDOPTS = -nostartfiles -nostdlib -Ttext

# Makefile targets
all: bootblock createimage kernel image boch_image

kernel: kernel.o
	$(LD) $(LDOPTS) $(KERNEL_ADDR) -o kernel $<

bootblock: bootblock.o
	$(LD) $(LDOPTS) 0x0 -o bootblock $<

createimage: createimage.o
	$(CC) -o createimage $<
	
# Create an image to put on the floppy
image: bootblock createimage kernel
	./createimage ./bootblock ./kernel

# Put the image on the floppy (these two stages are independent, as both
# vmware and bochs can run using only the image file stored on the harddisk)
#boot: image
#	cat ./image > /dev/sda

#write image to boch disk image
boch_image: image
	dd if=image of=bochs.img conv=notrunc

# Clean up!
clean:
	rm -f *.o
	rm -f createimage image bootblock kernel 

# No, really, clean up!
distclean: clean
	rm -f *~
	rm -f \#*
	rm -f *.bak
	rm -f bochsout.txt

# How to compile a C file
%.o:%.c
	$(CC) $(CCOPTS) $<

# How to assemble
%.o:%.s
	$(CC) $(CCOPTS) $<

# How to produce assembler input from a C file
%.s:%.c
	$(CC) $(CCOPTS) -S $<
