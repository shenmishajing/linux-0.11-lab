include ../Makefile.head

LDFLAGS	+= -Ttext 0 -e _start

KERNEL_SIZE = $(shell wc -c ../kernel.bin | tr -C -d '[0-9]')
   SYS_SIZE = 0x$(shell echo 'obase=16;($(KERNEL_SIZE)/1024/256+1)*1024*16' | bc)

all: bootsect setup

bootsect: bootsect.s ../kernel.bin
	$(Q)$(AS) $(AFLAGS) -o bootsect.o bootsect.s --defsym SYS_SIZE=$(SYS_SIZE)
	$(Q)$(LD) $(LDFLAGS) -o bootsect bootsect.o
	$(Q)cp -f bootsect bootsect.sym
	$(Q)nm bootsect.sym | grep -v '\(compiled\)\|\(\.o$$\)\|\( [aU] \)\|\(\.\.ng$$\)\|\(LASH[RL]DI\)'| sort > bootsect.map
	$(Q)$(OBJCOPY) -R .pdr -R .comment -R.note -S -O binary bootsect

setup: setup.s
	$(Q)$(AS) $(AFLAGS) -o setup.o setup.s
	$(Q)$(LD) $(LDFLAGS) -o setup setup.o
	$(Q)cp -f setup setup.sym
	$(Q)nm setup.sym | grep -v '\(compiled\)\|\(\.o$$\)\|\( [aU] \)\|\(\.\.ng$$\)\|\(LASH[RL]DI\)'| sort > setup.map
	$(Q)$(OBJCOPY) -R .pdr -R .comment -R.note -S -O binary setup

head.o: head.s
	$(Q)$(AS) $(AFLAGS) -o head.o head.s

clean:
	$(Q)rm -f bootsect bootsect.o setup setup.o head.o
