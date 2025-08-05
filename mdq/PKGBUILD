# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=mdq
pkgver=0.8.0
pkgrel=1
pkgdesc="like jq but for Markdown: find specific elements in a md doc"
arch=(x86_64)
url=https://github.com/yshavit/mdq
license=(Apache-2.0 MIT)
depends=(gcc-libs glibc)
makedepends=(cargo)
source=($url/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('6120705d573c5fc6189737bd584a1f2eff7f76fa94059ada78b207e248534d5b')

prepare() {
  cd $pkgname-$pkgver
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  cd $pkgname-$pkgver
  cargo build --all-features --frozen --release
}

check() {
  cd $pkgname-$pkgver
  cargo test --all-features --frozen
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 target/release/mdq "$pkgdir/usr/bin/mdq"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 LICENSE-MIT "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
