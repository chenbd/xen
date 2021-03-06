XEN_ROOT = $(CURDIR)/../../..
include $(XEN_ROOT)/tools/Rules.mk

# Xen script dir and scripts to go there.
XEN_SCRIPTS = vif-bridge
XEN_SCRIPTS += vif-route
XEN_SCRIPTS += vif-nat
XEN_SCRIPTS += vif-openvswitch
XEN_SCRIPTS += vif2
XEN_SCRIPTS += vif-setup
XEN_SCRIPTS-$(CONFIG_LIBNL) += remus-netbuf-setup
XEN_SCRIPTS += block
XEN_SCRIPTS += block-enbd block-nbd
XEN_SCRIPTS += external-device-migrate
XEN_SCRIPTS += vscsi
XEN_SCRIPTS += block-iscsi
XEN_SCRIPTS += block-tap
XEN_SCRIPTS += block-drbd-probe
XEN_SCRIPTS += block-dummy
XEN_SCRIPTS += $(XEN_SCRIPTS-y)
XEN_SCRIPTS += colo-proxy-setup
XEN_SCRIPTS += launch-xenstore

SUBDIRS-$(CONFIG_SYSTEMD) += systemd

XEN_SCRIPT_DATA = xen-script-common.sh locking.sh logging.sh
XEN_SCRIPT_DATA += xen-hotplug-common.sh xen-network-common.sh vif-common.sh
XEN_SCRIPT_DATA += block-common.sh

.PHONY: all
all: subdirs-all

.PHONY: build
build:

.PHONY: install
install: install-initd install-scripts subdirs-install

.PHONY: uninstall
uninstall: uninstall-initd uninstall-scripts subdirs-uninstall

# See docs/misc/distro_mapping.txt for INITD_DIR location
.PHONY: install-initd
install-initd:
	[ -d $(DESTDIR)$(INITD_DIR) ] || $(INSTALL_DIR) $(DESTDIR)$(INITD_DIR)
	[ -d $(DESTDIR)$(SYSCONFIG_DIR) ] || $(INSTALL_DIR) $(DESTDIR)$(SYSCONFIG_DIR)
	[ -d $(DESTDIR)$(LIBEXEC_BIN) ] || $(INSTALL_DIR) $(DESTDIR)$(LIBEXEC_BIN)
	$(INSTALL_DATA) init.d/sysconfig.xendomains $(DESTDIR)$(SYSCONFIG_DIR)/xendomains
	$(INSTALL_DATA) init.d/sysconfig.xencommons $(DESTDIR)$(SYSCONFIG_DIR)/xencommons
	$(INSTALL_PROG) xendomains $(DESTDIR)$(LIBEXEC_BIN)
	$(INSTALL_PROG) init.d/xendomains $(DESTDIR)$(INITD_DIR)
	$(INSTALL_PROG) init.d/xencommons $(DESTDIR)$(INITD_DIR)
	$(INSTALL_PROG) init.d/xendriverdomain $(DESTDIR)$(INITD_DIR)
	$(INSTALL_PROG) init.d/xen-watchdog $(DESTDIR)$(INITD_DIR)

.PHONY: uninstall-initd
uninstall-initd:
	rm -f $(DESTDIR)$(INITD_DIR)/xen-watchdog
	rm -f $(DESTDIR)$(INITD_DIR)/xendriverdomain
	rm -f $(DESTDIR)$(INITD_DIR)/xencommons
	rm -f $(DESTDIR)$(INITD_DIR)/xendomains
	rm -f $(DESTDIR)$(LIBEXEC_BIN)/xendomains
	rm -f $(DESTDIR)$(SYSCONFIG_DIR)/xencommons
	rm -f $(DESTDIR)$(SYSCONFIG_DIR)/xendomains

.PHONY: install-scripts
install-scripts:
	[ -d $(DESTDIR)$(XEN_SCRIPT_DIR) ] || \
		$(INSTALL_DIR) $(DESTDIR)$(XEN_SCRIPT_DIR)
	set -e; for i in $(XEN_SCRIPTS); \
	    do \
	    $(INSTALL_PROG) $$i $(DESTDIR)$(XEN_SCRIPT_DIR); \
	done
	set -e; for i in $(XEN_SCRIPT_DATA); \
	    do \
	    $(INSTALL_DATA) $$i $(DESTDIR)$(XEN_SCRIPT_DIR); \
	done

.PHONY: uninstall-scripts
uninstall-scripts:
	rm -f $(addprefix $(DESTDIR)$(XEN_SCRIPT_DIR)/, $(XEN_SCRIPTS))
	rm -f $(addprefix $(DESTDIR)$(XEN_SCRIPT_DIR)/, $(XEN_SCRIPT_DATA))

.PHONY: clean
clean: subdirs-clean

.PHONY: distclean
distclean: clean
