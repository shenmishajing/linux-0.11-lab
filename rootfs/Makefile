include ../Makefile.header

_HDA_IMG := $(CURDIR)/$(HDA_IMG)
_FLP_IMG := $(CURDIR)/$(FLP_IMG)
_RAM_IMG := $(CURDIR)/$(RAM_IMG)

hda: $(HDA_IMG).xz
ifneq ($(_HDA_IMG), $(wildcard $(_HDA_IMG)))
	@tar Jvxf $< >/dev/null 2>&1
endif

flp: $(FLP_IMG).xz
ifneq ($(_FLP_IMG), $(wildcard $(_FLP_IMG)))
	@tar Jvxf $< >/dev/null 2>&1
endif

ramfs: $(RAM_IMG).xz
ifneq ($(_RAM_IMG), $(wildcard $(_RAM_IMG)))
	@tar Jvxf $< >/dev/null 2>&1
endif

EXAMPLES ?= $(CURDIR)/../examples/

HDA_DIR = $(CURDIR)/hda
hda-mount: hda
	mkdir -p $(HDA_DIR)
	sudo mount -o offset=1024 $(HDA_IMG) $(HDA_DIR)
hda-umount:
	sudo umount $(HDA_DIR)

hda-install: hda-mount
	sudo cp -r $(EXAMPLES) $(HDA_DIR)/usr/root/
	sync
	make -s hda-umount

FLP_DIR = $(CURDIR)/flp
flp-mount: flp
	mkdir -p $(FLP_DIR)
	sudo mount -t minix $(FLP_IMG) $(FLP_DIR)
flp-umount:
	sudo umount $(FLP_DIR)

flp-install: flp-mount
	sudo cp -r $(EXAMPLES) $(FLP_DIR)/usr/root/
	sync
	make -s flp-umount

RAMFS_DIR = $(CURDIR)/ramfs
ramfs-mount: ramfs
	mkdir -p $(RAMFS_DIR)
	sudo mount $(RAM_IMG) $(RAMFS_DIR)
ramfs-umount:
	sudo umount $(RAMFS_DIR)

ramfs-install: ramfs-mount
	sudo cp -r $(EXAMPLES) $(RAMFS_DIR)/usr/root/
	sync
	make -s ramfs-umount

clean:
	@rm -rf $(HDA_IMG) $(HDA_DIR)
	@rm -rf $(FLP_IMG) $(FLP_DIR)
	@rm -rf $(RAM_IMG) $(RAM_DIR)
