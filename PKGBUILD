# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: houmain <houmain at posteo dot net>

pkgname=keymapper
pkgver=5.3.1
pkgrel=2
pkgdesc="A cross-platform context-aware keyremapper"
arch=(i686 x86_64 armv7h aarch64)
url=https://github.com/houmain/keymapper
license=(GPL-3.0-or-later)
depends=(dbus gcc-libs libappindicator-gtk3 libusb libx11 libxkbcommon
         libxkbcommon-x11 wayland)
makedepends=(cmake)
source=(${url}/archive/$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('e1ff17fa751caf00bbf3eae461f30cef6f6f9f830bd02a924de9a5efc0014b27')

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
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
