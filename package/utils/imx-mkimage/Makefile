# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright 2022 NXP
#
include $(TOPDIR)/rules.mk

PKG_NAME:=imx-mkimage
PKG_VERSION:=lf-5.10.52-2.1.0
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://source.codeaurora.org/external/imx/imx-mkimage.git
PKG_SOURCE_VERSION:=lf-5.10.52-2.1.0

PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/imx-mkimage
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Combine uboot and other firmware together
  BUILD_TARGET:=imx
  BUILD_SUBTARGET:=imx8
  DEPENDS:=@TARGET_imx_imx8
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR_IMAGE)
	$(CP) -rp $(PKG_BUILD_DIR) $(STAGING_DIR_IMAGE)
endef

define Build/Compile
endef

$(eval $(call BuildPackage,imx-mkimage))
