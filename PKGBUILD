# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=asciinema-agg
pkgver=1.4.0
pkgrel=1
pkgdesc="asciinema gif generator"
arch=(x86_64)
url=https://github.com/asciinema/agg
license=(Apache)
depends=(gcc-libs)
makedepends=(cargo)
source=(https://github.com/asciinema/agg/archive/v$pkgver/agg-$pkgver.tar.gz)
sha256sums=('0eb9c4b664fb4c81b7b58b574f6876fdc648ce57da36d7d168365b7fbccb488b')

build() {
  cd agg-$pkgver
  cargo build --release --locked
}

check() {
  cd agg-$pkgver
  cargo test --all --release --locked
}

package() {
  cd agg-$pkgver
  install -Dm0755 target/release/agg "$pkgdir/usr/bin/agg"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}
