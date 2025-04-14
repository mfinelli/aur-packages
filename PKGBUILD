# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Sabart Otto <seberm at gmail dot com>

pkgname=adminer
pkgver=5.2.1
pkgrel=1
pkgdesc="A full-featured MySQL management tool written in PHP"
arch=(any)
license=(Apache-2.0 GPL-2.0-only)
depends=(php-interpreter)
optdepends=(apache elasticsearch mariadb mongodb mysql postgresql sqlite)
url=https://www.adminer.org
install=adminer.install
source=("https://github.com/vrana/adminer/releases/download/v${pkgver}/adminer-${pkgver}.php")
sha256sums=('572118dff2305f136a4b8d59404d01c3d8450e99e3abd506081ffdddd8816573')

package() {
  install -Dm0644 "${pkgname}-${pkgver}.php" \
    "$pkgdir/usr/share/webapps/$pkgname/index.php"
}

# vim: set ts=2 sw=2 et:
