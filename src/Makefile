# This is a basic Makefile for setting the general configuration
include Makefile.head

LDFLAGS	+= -Ttext 0 -e startup_32
CFLAGS	+= -Iinclude
CPP	+= -Iinclude

#
# ROOT_DEV specifies the default root-device when making the image.
# This can be either FLOPPY, /dev/xxxx or empty, in which case the
# default of hd1(0301) is used by 'build'.
#
#ROOT_DEV= 021d	# FLOPPY B
#ROOT_DEV= 0301	# hd1

ARCHIVES=kernel/kernel.o mm/mm.o fs/fs.o
DRIVERS =kernel/blk_drv/blk_drv.a kernel/chr_drv/chr_drv.a
MATH	=kernel/math/math.a
LIBS	=lib/lib.a

.c.s:
	@$(CC) $(CFLAGS) -S -o $*.s $<
.s.o:
	@$(AS)  -o $*.o $<
.c.o:
	@$(CC) $(CFLAGS) -c -o $*.o $<

all:	Image

Image: boot/bootsect boot/setup kernel.sym FORCE
	@cp -f images/kernel.sym images/kernel.tmp
	@$(STRIP) images/kernel.tmp
	@$(OBJCOPY) -O binary -R .note -R .comment images/kernel.tmp images/kernel
	@$(BUILD) boot/bootsect boot/setup images/kernel images/Image
	@rm images/kernel.tmp
	@rm -f images/kernel
	@sync

init/main.o: FORCE
	@make main.o -s -C init/

boot/head.o: boot/head.s FORCE
	@make head.o -s -C boot/

kernel.sym: boot/head.o init/main.o \
		$(ARCHIVES) $(DRIVERS) $(MATH) $(LIBS)
	@$(LD) $(LDFLAGS) boot/head.o init/main.o \
	$(ARCHIVES) \
	$(DRIVERS) \
	$(MATH) \
	$(LIBS) \
	-o images/kernel.sym
	@nm images/kernel.sym | grep -v '\(compiled\)\|\(\.o$$\)\|\( [aU] \)\|\(\.\.ng$$\)\|\(LASH[RL]DI\)'| sort > images/kernel.map

kernel/math/math.a: FORCE
	@make -s -C kernel/math

kernel/blk_drv/blk_drv.a: FORCE
	@make -s -C kernel/blk_drv

kernel/chr_drv/chr_drv.a: FORCE
	@make -s -C kernel/chr_drv

kernel/kernel.o: FORCE
	@make -s -C kernel

mm/mm.o: FORCE
	@make -s -C mm

fs/fs.o: FORCE
	@make -s -C fs

lib/lib.a: FORCE
	@make -s -C lib

boot/setup: boot/setup.s FORCE
	@make setup -s -C boot

boot/bootsect: boot/bootsect.s FORCE
	@make bootsect -s -C boot

clean:
	@rm -f images/Image images/kernel.map tmp_make core boot/bootsect boot/setup
	@rm -f images/kernel.sym boot/*.o typescript* info bochsout.txt
	@for i in init mm fs kernel lib boot; do make clean -s -C $$i; done

distclean: clean
	@rm -f tag* cscope* linux-0.11.*

dep:
	@sed '/\#\#\# Dependencies/q' < Makefile > tmp_make
	@cp tmp_make Makefile
	@for i in init fs kernel mm; do make dep -s -C $$i; done

FORCE: ;
