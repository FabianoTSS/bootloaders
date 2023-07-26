LINUX_DIR := $(shell pwd)
TPLINK_BOARD_ID := TL-WR941HP-V2
BOOTL_FACTORY := CUSTOM
BOOTL_LIST := "https://raw.githubusercontent.com/FabianoTSS/bootloaders/main/bootloader_list.json"

define get-bootloader
	$(eval GET_LINK := $(shell wget -qO- $(1) | grep -o '"$(2)" *: *{[^}]*}' | grep -o '"DOWNLOAD_LINK" *: *"[^"]*"' | cut -d '"' -f 4))
	$(eval GET_HASH := $(shell wget -qO- $(1) | grep -o '"$(2)" *: *{[^}]*}' | grep -o '"SHA256SUM" *: *"[^"]*"' | cut -d '"' -f 4))
	$(shell wget -q -O $(2)_bootloader.bin $(GET_LINK))
	$(eval FILE_HASH := $(shell sha256sum $(2)_bootloader.bin | cut -d' ' -f1))
	@echo "Obtained Hash: $(FILE_HASH)"
	@echo "Expected Hash: $(GET_HASH)"
	@if [ "$(GET_HASH)" = "$(FILE_HASH)" ]; then \
		echo "Hashes match. Bootloader downloaded successfully!"; \
	else \
		echo "Hashes do not match. Deleting the file."; \
		rm -f $(2)_bootloader.bin; \
	fi
endef


all:
	$(call get-bootloader,$(BOOTL_LIST),$(TPLINK_BOARD_ID)-$(BOOTL_FACTORY))
