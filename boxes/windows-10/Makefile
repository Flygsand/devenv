ROOT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

install: .out/windows_10_virtualbox.box
	vagrant box add windows-10 .out/windows_10_virtualbox.box

clean:
	rm -rf .out

distclean: clean
	rm -rf .cache

.out/windows_10_virtualbox.box: .cache/Win10.iso
	mkdir -p .out
	cd ../../vendor/packer-windows; \
		packer build \
		-only=virtualbox-iso \
		-var iso_checksum=60cce9e9c6557335b4f7b18d02cfe2b438a8b3e2 \
		-var iso_url=$(ROOT_DIR)/.cache/Win10.iso \
		-var autounattend=$(ROOT_DIR)/Autounattend.xml \
		windows_10.json
	mv ../../vendor/packer-windows/windows_10_virtualbox.box .out

.cache/Win10.iso:
	$(eval iso_url := $(shell stty -echo; read -p "Windows 10 ISO URL: " iso_url; stty echo; echo $$iso_url))
	@echo
	mkdir -p .cache
	curl -L -o .cache/Win10.iso "$(iso_url)"

.PHONY: build install clean distclean
