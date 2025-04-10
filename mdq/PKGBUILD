# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=mdq
pkgver=0.5.0
pkgrel=1
pkgdesc="like jq but for Markdown: find specific elements in a md doc"
arch=(x86_64)
url=https://github.com/yshavit/mdq
license=(Apache-2.0 MIT)
depends=(gcc-libs)
makedepends=(cargo)
source=($url/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('9b1021bf710a1028d534769f6cbf035b199a6fc8a24607f2dbe9f3b18a7146d1')

build() {
  cd $pkgname-$pkgver
  cargo build --release --locked
}

check() {
  cd $pkgname-$pkgver
  cargo test --all --release --locked
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 target/release/mdq "$pkgdir/usr/bin/mdq"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 LICENSE-MIT "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
