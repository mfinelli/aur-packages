# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: houmain <houmain at posteo dot net>

pkgname=keymapper
pkgver=4.4.4
pkgrel=1
pkgdesc="A cross-platform context-aware keyremapper"
arch=(i686 x86_64 armv7h aarch64)
url=https://github.com/houmain/keymapper
license=(GPL-3.0-or-later)
depends=(dbus gcc-libs libappindicator-gtk3 libusb libx11 libxkbcommon wayland)
makedepends=(cmake)
source=(${url}/archive/$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('2b775dddfff726654d4bbd3faa779bfb79c05cc7ee142acceea157a841c35730')

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
