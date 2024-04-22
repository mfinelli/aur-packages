# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Taylon Silva <taylonsilva@gmail.com>
# Contributor: Andy Weidenbaum <archbaum@gmail.com>

pkgname=nodejs-n
pkgver=9.2.3
pkgrel=1
pkgdesc="Node version management"
arch=(any)
depends=(bash)
url=https://github.com/tj/n
license=(MIT)
install=$pkgname.install
source=(https://github.com/tj/n/archive/v$pkgver/n-$pkgver.tar.gz)
sha256sums=('c160fd30281d2aeb07d0101758f2f592dd3c6a23370417d9c6bf1efb5368a7dd')

package() {
  cd n-$pkgver

  PREFIX="$pkgdir/usr" make install

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}
