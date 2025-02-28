# Maintainer: Connor Behan <connor.behan@gmail.com>
# Contributor: Eric Bélanger <eric@archlinux.org>

pkgname=glib
pkgver=1.2.10
pkgrel=18
pkgdesc="Common C routines used by Gtk+ and other libs"
arch=('i686' 'x86_64' 'armv6h' 'aarch64')
url="http://www.gtk.org/"
license=('LGPL')
depends=('glibc' 'sh')
options=('!makeflags')
source=(https://download.gnome.org/sources/glib/1.2/${pkgname}-${pkgver}.tar.gz
	gcc340.patch aclocal-fixes.patch glib1-autotools.patch)
sha1sums=('e5a9361c594608d152d5d9650154c2e3260b87fa'
          'a2cc224a66aeffdcac16ebd9e8af18143cf54918'
          'ae4438cf56c0c9264ee36f6973fb445f9a820be0'
          '8a25fde3c79567262b3024f4e74c9ca4ee8a6279')

prepare() {
  cd ${pkgname}-${pkgver}
  patch -Np1 -i "${srcdir}/gcc340.patch"
  patch -Np0 -i "${srcdir}/aclocal-fixes.patch"
  patch -Np1 -i "${srcdir}/glib1-autotools.patch"
  sed -i -e 's/ifdef[[:space:]]*__OPTIMIZE__/if 0/' glib.h
  rm acinclude.m4
}

build() {
  cd ${pkgname}-${pkgver}
  if [[ $CARCH = "i686" ]]; then
    CONFIGFLAG='--host=i686-pc-linux-gnu --target=i686-pc-linux-gnu'
  elif [[ $CARCH = "x86_64" ]]; then
    CONFIGFLAG='--host=x86_64-unknown-linux-gnu --target=x86_64-unknown-linux-gnu'
  elif [[ $CARCH = "armv6h" ]]; then
    CONFIGFLAG='--host=armv6l-unknown-linux-gnueabihf --target=armv6l-unknown-linux-gnueabihf'
  fi

  autoreconf --force --install
  CFLAGS="-Wno-format-security" ./configure --prefix=/usr --mandir=/usr/share/man \
    --infodir=/usr/share/info $CONFIGFLAG
  make
}

check() {
  cd ${pkgname}-${pkgver}
  make check
}

package() {
  cd ${pkgname}-${pkgver}
  make DESTDIR="${pkgdir}" install
}
