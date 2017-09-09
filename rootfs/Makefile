include ../Makefile.header

HDA_IMG_FLAG := $(shell find ./ -name $(HDA_IMG))
FLP_IMG_FLAG := $(shell find ./ -name $(FLP_IMG))
RAM_IMG_FLAG := $(shell find ./ -name $(RAM_IMG))

hda: $(HDA_IMG).xz
ifeq ($(HDA_IMG_FLAG),)
	@tar Jvxf $< >/dev/null 2>&1
endif

flp: $(FLP_IMG).xz
ifeq ($(FLP_IMG_FLAG),)
	@tar Jvxf $< >/dev/null 2>&1
endif

ramfs: $(RAM_IMG).xz
ifeq ($(RAM_IMG_FLAG),)
	@tar Jvxf $< >/dev/null 2>&1
endif

EXAMPLES ?= $(CURDIR)/../examples/

HDA_DIR = $(CURDIR)/hda
hda-install: hda
	mkdir -p $(HDA_DIR)
	sudo mount -o offset=1024 $(HDA_IMG) $(HDA_DIR)
	sudo cp -r $(EXAMPLES) $(HDA_DIR)/usr/root/
	sync
	sudo umount $(HDA_DIR)

FLP_DIR = $(CURDIR)/flp
flp-install: flp
	mkdir -p $(FLP_DIR)
	sudo mount -t minix $(FLP_IMG) $(FLP_DIR)
	sudo cp -r $(EXAMPLES) $(FLP_DIR)/usr/root/
	sync
	sudo umount $(FLP_DIR)

RAMFS_DIR = $(CURDIR)/ramfs
ramfs-install: ramfs
	mkdir -p $(RAMFS_DIR)
	sudo mount $(RAM_IMG) $(RAMFS_DIR)
	sudo cp -r $(EXAMPLES) $(RAMFS_DIR)/usr/root/
	sync
	sudo umount $(RAMFS_DIR)

clean:
	@rm -rf $(HDA_IMG) $(HDA_DIR)
	@rm -rf $(FLP_IMG) $(FLP_DIR)
	@rm -rf $(RAM_IMG) $(RAM_DIR)
