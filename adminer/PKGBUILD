# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Sabart Otto <seberm at gmail dot com>

pkgname=adminer
pkgver=5.0.2
pkgrel=1
pkgdesc="A full-featured MySQL management tool written in PHP"
arch=(any)
license=(Apache-2.0 GPL-2.0-only)
depends=(php)
optdepends=(apache elasticsearch mariadb mongodb mysql postgresql sqlite)
url=https://www.adminer.org
install=adminer.install
source=("https://github.com/vrana/adminer/releases/download/v${pkgver}/adminer-${pkgver}.php")
sha256sums=('c8f645ddc20d379e53c761c7c11d65a243521bd08dcb380ced811735e65843a8')

package() {
  cd "$srcdir"
  install -Dm0644 "${pkgname}-${pkgver}.php" \
    "$pkgdir/usr/share/webapps/$pkgname/index.php"
}

# vim: set ts=2 sw=2 et:
