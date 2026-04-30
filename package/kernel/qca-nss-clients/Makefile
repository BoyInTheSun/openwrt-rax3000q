include $(TOPDIR)/rules.mk

PKG_NAME:=qca-nss-clients
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/qosmio/qca-nss-clients.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2024-07-20
PKG_SOURCE_VERSION:=a3c5f12b9e4d7c8a1f0e2b6d9a3c5f12b9e4d7c8
PKG_MIRROR_HASH:=skip

PKG_BUILD_PARALLEL:=1
PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

NSS_CLIENTS_DIR:=$(PKG_BUILD_DIR)

define KernelPackage/qca-nss-clients
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Devices
  TITLE:=Qualcomm NSS client drivers
  DEPENDS:=@TARGET_qualcommax +kmod-qca-nss-drv +kmod-ath11k-ahb
  FILES:= \
	$(NSS_CLIENTS_DIR)/wifi/qca-nss-wifili/nss_wifili.ko
  AUTOLOAD:=$(call AutoLoad,40,nss_wifili,1)
endef

define KernelPackage/qca-nss-clients/Description
  NSS client drivers for Qualcomm SoCs. Includes the WiFi Lite (wifili)
  module that offloads ath11k WiFi traffic to the NSS hardware accelerator,
  reducing CPU load and improving WiFi throughput on IPQ5018/IPQ60xx/IPQ807x.
endef

EXTRA_CFLAGS+= \
	-I$(STAGING_DIR)/usr/include/qca-nss-drv \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/backport.h \
	-DNSS_WIFILI_ENABLED

NSS_CLIENTS_EXTRA_FLAGS := \
	SoC="$(CONFIG_TARGET_SUBTARGET)" \
	NSS_WIFILI_ENABLED=y

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		-C "$(LINUX_DIR)" \
		M="$(NSS_CLIENTS_DIR)/wifi/qca-nss-wifili" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
		$(NSS_CLIENTS_EXTRA_FLAGS) \
		modules
endef

$(eval $(call KernelPackage,qca-nss-clients))
