#
# Copyright (C) 2008-2014 OpenWrt.org
# Copyright (C) 2016 LEDE project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=uci
PKG_RELEASE:=1

PKG_SOURCE_URL=$(PROJECT_GIT)/project/uci.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-01-17
PKG_SOURCE_VERSION:=fb3c2343b17b759b175f11aec5b3fbb1cf48bbc3
PKG_MIRROR_HASH:=c9302f4a1cb400134cb9fc0622fb6a04bbe8c55bcc83ec454caadb1e62f3257e

PKG_LICENSE:=LGPL-2.1
PKG_LICENSE_FILES:=

PKG_MAINTAINER:=Felix Fietkau <nbd@nbd.name>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

# set to 1 to enable debugging
DEBUG=

define Package/libuci
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=C library for the Unified Configuration Interface (UCI)
  DEPENDS:=+libubox
  ABI_VERSION:=20250117
endef

define Package/uci
  SECTION:=base
  CATEGORY:=Base system
  DEPENDS:=+libuci
  TITLE:=Utility for the Unified Configuration Interface (UCI)
endef

define Package/libuci-lua
  SECTION=libs
  CATEGORY=Libraries
  DEPENDS:=+libuci +liblua
  TITLE:=Lua plugin for UCI
endef

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib

CMAKE_OPTIONS += \
	-DLUAPATH=/usr/lib/lua \
	$(if $(DEBUG),-DUCI_DEBUG=ON)

define Package/libuci/install
	$(INSTALL_DIR) $(1)/lib
	$(CP) $(PKG_BUILD_DIR)/libuci.so* $(1)/lib/
endef

define Package/libuci-lua/install
	$(INSTALL_DIR) $(1)/usr/lib/lua
	$(CP) $(PKG_BUILD_DIR)/lua/uci.so $(1)/usr/lib/lua/
endef

define Package/uci/install
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/uci $(1)/sbin/
	$(CP) ./files/* $(1)/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_BUILD_DIR)/uci{,_config,_blob,map}.h $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libuci.so* $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libucimap.a $(1)/usr/lib
endef

$(eval $(call BuildPackage,libuci))
$(eval $(call BuildPackage,libuci-lua))
$(eval $(call BuildPackage,uci))
