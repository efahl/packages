include $(TOPDIR)/rules.mk

PKG_NAME:=external-protocol
PKG_VERSION:=20231119
PKG_RELEASE:=2

PKG_MAINTAINER:=Oskari Rauta <oskari.rauta@gmail.com>

include $(INCLUDE_DIR)/package.mk

define Package/external-protocol
  SECTION:=net
  CATEGORY:=Network
  TITLE:=externally managed protocol
  DEPENDS:=+ip
  PKGARCH:=all
endef

define Package/external-protocol/description
  external protocol is a general protocol for assisting
  setup of many virtual devices that lack proper
  protocol support in openwrt. Such as netavark, cni and
  netbird for example. External protocol is supposed
  to be managed with external software, not directly.

  external protocol works automaticly on the background
  and sets up netifd details when interface comes up or
  goes down. This allows one to easily add interface to
  a firewall zone.

  as a example use case, podman, with network where it's
  internal firewall and portmapper are disabled, control
  of firewalling, whether it was exposing ports or
  limiting/accepting access between networks, such as
  lan can be made through openwrt's own firewalling
  configuration if you used external protocol.

  podman example configuration could be as following:
    - lan network: 10.0.0.0/16 (255.255.0.0)
    - container network: 10.129.0.1/24 (255.255.255.0)

  Add a network configuration for your container network
  using external protocol. Then create firewall zone for it.

  You could create a new container/pod with static ip
  address 10.129.0.2 (as 10.129.0.1 as container network's
  gateway).

  Easily define permissions so that local networks can
  connect to container network, but not the other way around.
  Also you want to allow forwarding from/to wan.

  Now, as container cannot access local dns, make a rule for
  your firewall to accept connections from container network
  to port 53 (dns).

  Now all you have to do, is make redirects to your firewall
  and point them to 10.129.0.2 and connections from wan are
  redirectered to containers/pods.

  external protocol also works for other applications as
  well that are using veth/tun/etc devices and don't have
  a hand-tailored protocol available, such as vpn service
  netbird.

  Protocol has 3 settings: device, searchdomain and delay.
  Sometimes polling interfaces takes some time, and in
  that case you might want to add few seconds to delay.
  Otherwise, it can be excluded from configuration.
  Option for searchdomain is also completely optional.

  package was previously known as cni protocol but as
  it can be used on so many other things, naming became
  mis-leading and it was renamed to external protocol.
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/external-protocol/install
	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_BIN) ./files/external.sh $(1)/lib/netifd/proto/external.sh
endef

$(eval $(call BuildPackage,external-protocol))
