include $(TOPDIR)/rules.mk

PKG_NAME:=traefik
PKG_VERSION:=3.1.2
PKG_RELEASE:=1
PKG_LICENSE:=MIT
PKG_CPE_ID:=cpe:/a:traefik:traefik

PKG_SOURCE:=$(PKG_NAME)-v$(PKG_VERSION).src.tar.gz
PKG_SOURCE_URL:=https://github.com/traefik/traefik/releases/download/v$(PKG_VERSION)/
PKG_HASH:=d8cada1d42e2fad4cbe15b75e8db21647b520ffd49dd09814cc1131c3fe02d00

PKG_MAINTAINER:=Mariusz Fik <mariusz@fidano.pl>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

# This prevents OpenWrt from using the default unpacking behavior
PKG_UNPACK=mkdir -p $(PKG_BUILD_DIR) && $(TAR) -C $(PKG_BUILD_DIR) -xzvf $(DL_DIR)/$(PKG_SOURCE)

GO_PKG:=github.com/traefik/traefik/v3
GO_PKG_LDFLAGS:=-s -w
GO_PKG_LDFLAGS_X:=$(GO_PKG)/v3/pkg/version.Version=v$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/traefik
	SECTION:=net
	CATEGORY:=Network
	TITLE:=The Cloud Native Application Proxy
	URL:=https://traefik.io/
	DEPENDS:=$(GO_ARCH_DEPENDS) +ca-certificates
	USERID:=traefik=8000:traefik=8000
	MENU:=1
endef

define Package/traefik/description
Traefik (pronounced traffic) is a modern HTTP reverse proxy and load balancer
that makes deploying microservices easy. Traefik integrates with your existing
infrastructure components (Docker, Swarm mode, Kubernetes, Marathon, Consul,
Etcd, Rancher, Amazon ECS) and configures itself automatically and dynamically.

Pointing Traefik at your orchestrator should be the only configuration step you need.
endef

define Package/traefik/conffiles
/etc/traefik/
endef

define Package/traefik/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/traefik
	$(INSTALL_DIR) $(1)/etc/traefik/conf.d
	$(INSTALL_DIR) $(1)/etc/traefik/letsencrypt
	$(INSTALL_DIR) $(1)/etc/capabilities
	$(INSTALL_DIR) $(1)/var/log/traefik
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/traefik $(1)/usr/bin
	$(INSTALL_BIN) $(CURDIR)/files/traefik.init $(1)/etc/init.d/traefik
	$(INSTALL_CONF) $(CURDIR)/files/traefik.yml $(1)/etc/traefik
	$(INSTALL_DATA) $(CURDIR)/files/traefik.json $(1)/etc/capabilities
endef

$(eval $(call BuildPackage,traefik))
