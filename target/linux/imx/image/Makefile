# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright 2022 NXP

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

# The limitation of flash sysupgrade.bin is 1MB dtb + 16MB kernel + 32MB rootfs
IMX_SD_KERNELPART_SIZE = 40
IMX_SD_KERNELPART_OFFSET = 16
IMX_SD_ROOTFSPART_OFFSET = 64
IMX_SD_IMAGE_SIZE = $(shell echo $$((($(IMX_SD_ROOTFSPART_OFFSET) + \
	$(CONFIG_TARGET_ROOTFS_PARTSIZE)))))

define Image/Prepare
	# Build .dtb for all boards we may run on && generate flash.bin
	echo $(DEVICE_DTS)
	# echo end
endef

define Build/imx-compile-dtb
	# Compile dts file to dtb
	$(call Image/BuildDTB,$(DTS_DIR)/$(DEVICE_DTS).dts,$(DTS_DIR)/$(DEVICE_DTS).dtb)
endef

define Build/imx-create-flash
	# Combile firmware + bl31 + uboot to flash.bin
	cd $(STAGING_DIR_IMAGE)/$(MKIMG_DIR) && $(MAKE) SOC=$(PLAT) $(DEVICE_TYPE)
endef

define Build/imx-append-boot
	# Append the uboot, firmware etc.
	dd if=$(STAGING_DIR_IMAGE)/$(MKIMG_DIR)/$(SOC_TYPE)/flash.bin >> $@
endef

define Build/imx-clean
	# Clean the target
	rm -f $@
endef

define Build/imx-append
	# append binary
	dd if=$(STAGING_DIR_IMAGE)/$(1) >> $@
endef

define Build/imx-append-dtb
	# Append the dtb file
	dd if=$(DTS_DIR)/$(1).dtb >> $@
endef

define Build/imx-append-kernel
	# append the kernel
	mkdir -p $@.tmp && \
	cp $(IMAGE_KERNEL) $@.tmp && \
	cp $(DTS_DIR)/$(DEVICE_DTS).dtb $@.tmp && \
	make_ext4fs -J -L kernel -l "$(IMX_SD_KERNELPART_SIZE)M" "$@.kernel.part" "$@.tmp" && \
	dd if=$@.kernel.part >> $@ && \
	rm -rf $@.tmp && \
	rm -f $@.kernel.part
endef

define Build/imx-append-sdhead
	# Create the sd file table
	./gen_sdcard_head_img.sh $(STAGING_DIR_IMAGE)/$(1)-sdcard-head.img \
		$(IMX_SD_KERNELPART_OFFSET) $(IMX_SD_KERNELPART_SIZE) \
		$(IMX_SD_ROOTFSPART_OFFSET) $(CONFIG_TARGET_ROOTFS_PARTSIZE)
	dd if=$(STAGING_DIR_IMAGE)/$(1)-sdcard-head.img >> $@
endef

include $(SUBTARGET).mk

$(eval $(call BuildImage))
