# Maintainer: Mario Finelli

pkgname=plymouth-theme-arch-logo-symbol
pkgver=1
pkgrel=1
pkgdesc="Arch linux logo (symbol) plymouth theme"
arch=(any)
url=https://github.com/mfinelli/plymouth-theme-arch-logo-symbol
license=(MIT MIT-0 X11)
depends=(plymouth)
source=(${url}/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('f72e62406819ef07c7d678cf54d0b2b2ce4a26e956695a1542abcbfa4119975a')

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
}

# vim: set ts=2 sw=2 et:
