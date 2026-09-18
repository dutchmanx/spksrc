PKG_NAME = libhdhomerun
PKG_VERS = 20260313
PKG_EXT = tgz
PKG_DIST_NAME = $(PKG_NAME)_$(PKG_VERS).$(PKG_EXT)
PKG_DIST_SITE = https://download.silicondust.com/hdhomerun
PKG_DIR = $(PKG_NAME)

DEPENDS =

HOMEPAGE = https://www.silicondust.com/hdhomerun/
COMMENT = HDHomerun Library
LICENSE = LGPL v2.1

CONFIGURE_TARGET = nop
INSTALL_TARGET = libhdhomerun_install

include ../../mk/spksrc.cross-cc.mk

# Upstream derives its tools from CROSS_COMPILE, a single prefix, which cannot name an
# overlay compiler: that one sits in another directory and carries a version suffix. The
# tools it reads by name are given by name, and win on the make command line; the prefix
# stays behind them, from TC_PATH rather than rebuilt here, for whatever else it derives.
COMPILE_ARGS = CROSS_COMPILE=$(TC_PATH)$(TC_PREFIX)
COMPILE_ARGS += CC=$(call tc,gcc) AR=$(call tc,ar) STRIP=$(call tc,strip)

.PHONY: libhdhomerun_install
libhdhomerun_install:
	@install -m 755 -d $(STAGING_INSTALL_PREFIX)/include/libhdhomerun
	@install -m 644 $(INSTALL_DIR)/../$(PKG_DIR)/*.h $(STAGING_INSTALL_PREFIX)/include/libhdhomerun/
	@install -m 755 -d $(STAGING_INSTALL_PREFIX)/bin
	@install -m 755 $(INSTALL_DIR)/../$(PKG_DIR)/hdhomerun_config $(STAGING_INSTALL_PREFIX)/bin/
	@install -m 755 -d $(STAGING_INSTALL_PREFIX)/lib
	@install -m 644 $(INSTALL_DIR)/../$(PKG_DIR)/libhdhomerun.so $(STAGING_INSTALL_PREFIX)/lib/
