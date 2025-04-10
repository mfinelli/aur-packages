# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Sabart Otto <seberm at gmail dot com>

pkgname=adminer
pkgver=5.2.0
pkgrel=1
pkgdesc="A full-featured MySQL management tool written in PHP"
arch=(any)
license=(Apache-2.0 GPL-2.0-only)
depends=(php-interpreter)
optdepends=(apache elasticsearch mariadb mongodb mysql postgresql sqlite)
url=https://www.adminer.org
install=adminer.install
source=("https://github.com/vrana/adminer/releases/download/v${pkgver}/adminer-${pkgver}.php")
sha256sums=('d6ae5bfa46263ab2ee16e80d66fc6d5300bc38361210a54019354e192714d73e')

package() {
  install -Dm0644 "${pkgname}-${pkgver}.php" \
    "$pkgdir/usr/share/webapps/$pkgname/index.php"
}

# vim: set ts=2 sw=2 et:
