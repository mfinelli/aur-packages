# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Taylon Silva <taylonsilva@gmail.com>
# Contributor: Andy Weidenbaum <archbaum@gmail.com>

pkgname=nodejs-n
pkgver=10.1.0
pkgrel=1
pkgdesc="Node version management"
arch=(any)
depends=(bash)
optdepends=("jq: faster package.json lookups")
url=https://github.com/tj/n
license=(MIT)
install=$pkgname.install
source=(https://github.com/tj/n/archive/v$pkgver/n-$pkgver.tar.gz)
sha256sums=('53f686808ef37728922ad22e8a5560f4caf1d214d706639ef8eca6e72b891697')

package() {
  cd n-$pkgver

  PREFIX="$pkgdir/usr" make install

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}
