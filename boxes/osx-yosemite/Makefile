ROOT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
OSX_RELEASE := 10.10.5
OSX_BUILD := 14F27

install: .out/packer_virtualbox-iso_virtualbox.box
	vagrant box add osx-yosemite out/packer_virtualbox-iso_virtualbox.box

clean: 
	rm -rf .out

distclean: clean
	rm -rf .cache ../../vendor/osx-vm-templates/packer/packer_cache

.out/packer_virtualbox-iso_virtualbox.box: .cache/OSX_InstallESD_$(OSX_RELEASE)_$(OSX_BUILD).dmg
	cd ../../vendor/osx-vm-templates/packer; \
		packer build \
		-only=virtualbox-iso \
		-var iso_checksum=$(shell cat .cache/OSX_InstallESD_$(OSX_RELEASE)_$(OSX_BUILD).dmg.md5) \
		-var iso_url=$(ROOT_DIR)/.cache/OSX_InstallESD_$(OSX_RELEASE)_$(OSX_BUILD).dmg \
		-var autologin=true \
		template.json
	mv ../../vendor/osx-vm-templates/packer/packer_virtualbox-iso_virtualbox.box out

.cache/OSX_InstallESD_10.10.5_14F27.dmg:
	mkdir -p .cache
	sudo ../../vendor/osx-vm-templates/prepare_iso/prepare_iso.sh "/Applications/Install OS X Yosemite.app" .cache | tee /dev/stderr | perl -pe 's/\x1b\[[0-9;]*m//g' | awk '/-- MD5: / {printf $$NF}' > .cache/OSX_InstallESD_$(OSX_RELEASE)_$(OSX_BUILD).dmg.md5

.PHONY: install clean distclean
