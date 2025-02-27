# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright 2022 NXP
#

include $(TOPDIR)/rules.mk

PKG_NAME:=atf-imx
PKG_VERSION:=imx_5.4.70_2.3.0
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://source.codeaurora.org/external/imx/imx-atf
PKG_SOURCE_VERSION:=imx_5.4.70_2.3.0
PKG_MIRROR_HASH:=127b063a4f5092c09c65e43fded3b98d88248814042e2af754cc7faa291e7ef8
PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

HOST_CFLAGS += -Wall -Werror -pedantic -std=c99
define Host/Compile
endef

define Host/Install
endef

define Package/atf-imx8mp
  SECTION:=boot
  CATEGORY:=Boot Loaders
  TITLE:=NXP i.MX8Mplus SD Boot Trusted Firmware
  PLAT:=imx8mp
  MKIMG_DIR:=`find $(STAGING_DIR_IMAGE) -name imx-mkimage*  | xargs basename`/iMX8M
  BUILD_TARGET:=imx
  BUILD_SUBTARGET:=imx8
  DEPENDS:=@TARGET_imx_imx8
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR_IMAGE)
	$(CP) $(PKG_BUILD_DIR)/build/$(PLAT)/release/bl31.bin \
		$(STAGING_DIR_IMAGE)/$(MKIMG_DIR)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) CROSS_COMPILE=$(TARGET_CROSS) \
		PLAT=$(PLAT) bl31
endef

$(eval $(call BuildPackage,atf-imx8mp))
