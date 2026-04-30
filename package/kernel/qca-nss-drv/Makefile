include $(TOPDIR)/rules.mk

PKG_NAME:=qca-nss-drv
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/qosmio/qca-nss-drv.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2024-07-20
PKG_SOURCE_VERSION:=e2b1d28a2aa4e1b7b3e2c4f3b5d1a9c0e7f8b2d4
PKG_MIRROR_HASH:=skip

PKG_BUILD_PARALLEL:=1
PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/qca-nss-drv
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Devices
  TITLE:=Qualcomm NSS core driver
  DEPENDS:=@TARGET_qualcommax
  FILES:=$(PKG_BUILD_DIR)/qca-nss-drv.ko
  AUTOLOAD:=$(call AutoLoad,32,qca-nss-drv,1)
endef

define KernelPackage/qca-nss-drv/Description
  NSS (Network Subsystem) core driver for Qualcomm SoCs.
  Provides the communication layer between the ARM application processor
  and the dedicated NSS hardware accelerator cores.
  Required by qca-nss-clients for WiFi offloading.
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include/qca-nss-drv
	$(CP) $(PKG_BUILD_DIR)/exports/* $(1)/usr/include/qca-nss-drv/ 2>/dev/null || true
	$(CP) $(PKG_BUILD_DIR)/include/* $(1)/usr/include/qca-nss-drv/ 2>/dev/null || true
endef

EXTRA_CFLAGS+= \
	-I$(PKG_BUILD_DIR)/exports \
	-I$(PKG_BUILD_DIR)/include

NSS_DRV_EXTRA_FLAGS := \
	SoC="$(CONFIG_TARGET_SUBTARGET)"

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		-C "$(LINUX_DIR)" \
		M="$(PKG_BUILD_DIR)" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
		$(NSS_DRV_EXTRA_FLAGS) \
		modules
endef

$(eval $(call KernelPackage,qca-nss-drv))
