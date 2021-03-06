#
# Copyright (C) 2006-2011 OpenWrt.org
# Copyright (C) 2017-2018 Luiz Angelo Daros de Luca <luizluca@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hplip
PKG_VERSION:=3.18.10
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/hplip
PKG_HASH:=50d3c645f4ba5ae4a10c0f87cedcd0c0fd1598b412c9511476a9bb3b13101901

PKG_MAINTAINER:=Luiz Angelo Daros de Luca <luizluca@gmail.com>
PKG_LICENSE:=GPL-2.0 GPL-2.0+
PKG_LICENSE_FILES:=COPYING LICENSE

PKG_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:= python libcups
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/hplip/Default
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing
  URL:=https://sourceforge.net/projects/hplip/
endef

define Package/hplip/Default/description
	HPLIP is an HP developed solution for printing, scanning, and faxing with HP inkjet and laser based printers in Linux.
endef

define Package/hplip-common
$(call Package/hplip/Default)
  TITLE+= (common files)
  DEPENDS+=+libusb-1.0
endef

define Package/hplip-common/description
$(call Package/hplip/Default/description)

These are common files shared between subpackages
endef

define Package/hplip-sane
$(call Package/hplip/Default)
  TITLE+= (scanner drivers)
  DEPENDS+=+libsane +hplip-common
endef

define Package/hplip-sane/description
$(call Package/hplip/Default/description)

S.A.N.E backend for HP Scanners
endef

CONFIGURE_ARGS += \
	--disable-gui-build \
	--disable-network-build \
	--disable-fax-build \
	--disable-pp-build \
	--disable-doc-build \
	--disable-dbus-build \
	--disable-hpijs-only-build \
	--disable-hpcups-install \
	--disable-hpps-install \
	--disable-cups-drv-install \
	--enable-lite-build

define Package/hplip-common/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpip.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpmud.so* $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/etc/hp
	$(CP) $(PKG_BUILD_DIR)/hplip.conf $(1)/etc/hp/hplip.conf

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/models/
	$(CP) $(PKG_BUILD_DIR)/data/models/models.dat $(1)/usr/share/hplip/data/models/

	$(INSTALL_DIR) $(1)/etc/hotplug.d/usb/
	$(INSTALL_BIN) ./files/20-hplip $(1)/etc/hotplug.d/usb/
endef

define Package/hplip-sane/install
	$(INSTALL_DIR) $(1)/usr/lib/sane
	$(CP) $(PKG_BUILD_DIR)/.libs/libsane-hpaio.so* $(1)/usr/lib/sane

	$(INSTALL_DIR) $(1)/etc/sane.d/dll.d/
	$(INSTALL_DATA) ./files/hplib.conf $(1)/etc/sane.d/dll.d/hplib
endef

define Package/hplip-common/conffiles
/etc/hp/hplip.conf
endef

define Package/hplip-sane/conffiles
/etc/sane.d/dll.d/hplib
endef

$(eval $(call BuildPackage,hplip-common))
$(eval $(call BuildPackage,hplip-sane))
