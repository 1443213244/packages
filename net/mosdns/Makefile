# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=mosdns
PKG_VERSION:=5.3.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/IrineSistiana/mosdns/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=1d7eeaa735cb48ed2d436797d7f2a82541699f74647cd293ee411a72cdc65f5f

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/IrineSistiana/mosdns
GO_PKG_LDFLAGS_X:=main.version=v$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/mosdns
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  TITLE:=A plug-in DNS forwarder/splitter
  URL:=https://github.com/IrineSistiana/mosdns
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
  PROVIDES:=mosdns-neo
endef

define Package/mosdns/conffiles
/etc/mosdns/
endef

define Package/mosdns/install
	$(call GoPackage/Package/Install/Bin,$(1))

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scripts/openwrt/mosdns-init-openwrt $(1)/etc/init.d/mosdns

	$(INSTALL_DIR) $(1)/etc/mosdns
	$(INSTALL_DATA) ./files/config.yaml $(1)/etc/mosdns/config.yaml

	$(INSTALL_DIR) $(1)/etc/mosdns/rules
	$(INSTALL_DATA) ./files/rules/chinalist.txt $(1)/etc/mosdns/rules/chinalist.txt
	$(INSTALL_DATA) ./files/rules/gfwlist.txt $(1)/etc/mosdns/rules/gfwlist.txt
	$(INSTALL_DATA) ./files/rules/local.txt $(1)/etc/mosdns/rules/local.txt
endef

define Package/mosdns/postinst
#!/bin/sh

# Enable the service to start on boot
/etc/init.d/mosdns enable

# Start the service
/etc/init.d/mosdns start

exit 0
endef

$(eval $(call GoBinPackage,mosdns))
$(eval $(call BuildPackage,mosdns))
