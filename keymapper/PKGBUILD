# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: houmain <houmain at posteo dot net>

pkgname=keymapper
pkgver=5.0.0
pkgrel=1
pkgdesc="A cross-platform context-aware keyremapper"
arch=(i686 x86_64 armv7h aarch64)
url=https://github.com/houmain/keymapper
license=(GPL-3.0-or-later)
depends=(dbus gcc-libs libappindicator-gtk3 libusb libx11 libxkbcommon
         libxkbcommon-x11 wayland)
makedepends=(cmake)
source=(${url}/archive/$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('0c9e4c69e5c1a18cd554113fb92504a986e18bcc4ebe4a798a96de49e9274509')

prepare() {
  cd $pkgname-$pkgver

  # use full path for execstart
  sed -i 's|^ExecStart=keymapperd|ExecStart=/usr/bin/keymapperd|' \
    extra/lib/systemd/system/keymapperd.service
}

build() {
  cd $pkgname-$pkgver
  cmake . -DVERSION="$pkgver" -DCMAKE_INSTALL_PREFIX=/usr
  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
}

# vim: set ts=2 sw=2 et:
