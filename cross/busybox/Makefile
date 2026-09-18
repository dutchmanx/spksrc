PKG_NAME = busybox
PKG_VERS = 1.35.0
PKG_EXT = tar.bz2
PKG_DIST_NAME = $(PKG_NAME)-$(PKG_VERS).$(PKG_EXT)
# use own source (busybox.net is slow and often unavailable)
PKG_DIST_SITE = https://github.com/SynoCommunity/spksrc/releases/download/sources
PKG_DIR = $(PKG_NAME)-$(PKG_VERS)

DEPENDS =

HOMEPAGE = https://busybox.net
COMMENT  = Fairly comprehensive set of programs needed to run a Linux system.
LICENSE  = GPL

CONFIGURE_TARGET = busybox_configure
INSTALL_TARGET = busybox_install

# Kbuild derives every tool from CROSS_COMPILE, a single prefix -- an overlay compiler sits
# in another directory and carries a version suffix, so no prefix can name it. Each tool is
# named instead, on the make command line rather than in the environment: kbuild assigns
# `CC = $(CROSS_COMPILE)gcc` in its own makefile, which an exported CC would not override.
# Handed to the configure step as well, where kbuild probes the compiler for its flags.
BUSYBOX_TOOLS  = CC=$(call tc,gcc) AS=$(call tc,as) AR=$(call tc,ar) LD=$(call tc,ld) NM=$(call tc,nm)
BUSYBOX_TOOLS += STRIP=$(call tc,strip) OBJCOPY=$(call tc,objcopy) OBJDUMP=$(call tc,objdump)
COMPILE_ARGS += $(BUSYBOX_TOOLS)

include ../../mk/spksrc.cross-cc.mk

# WARNING: without BUSYBOX_CONFIG definitions, the build does not work
# Activate at least one config to build in cross/busybox
#BUSYBOX_CONFIG = procutils

.PHONY: busybox_configure
busybox_configure:
	@$(MSG) "Configure for busybox config: $(BUSYBOX_CONFIG)"
	cp spksrc_defconfig $(WORK_DIR)/$(PKG_DIR)/configs/spksrc_defconfig
	cat $(addprefix configs/, $(BUSYBOX_CONFIG)) | while IFS== read option value; do \
	  sed -r -e "s/^(# )?$$option\s.*$$/$$option=$$value/" -i $(WORK_DIR)/$(PKG_DIR)/configs/spksrc_defconfig ; \
	done
	$(RUN) $(MAKE) $(BUSYBOX_TOOLS) spksrc_defconfig

.PHONY: busybox_install
busybox_install:
	@$(MSG) Install busybox binary
	install -m 755 -d $(STAGING_INSTALL_PREFIX)/bin
	install -m 755 $(WORK_DIR)/$(PKG_DIR)/busybox $(STAGING_INSTALL_PREFIX)/bin
