# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=mdq
pkgver=0.3.0
pkgrel=1
pkgdesc="like jq but for Markdown: find specific elements in a md doc"
arch=(x86_64)
url=https://github.com/yshavit/mdq
license=(Apache-2.0 MIT)
depends=(gcc-libs)
makedepends=(cargo)
source=($url/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('0a62a73f762e6ffa38dc914c585916d50d093c282a4805d6abd3cf1896a14293')

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
