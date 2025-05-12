# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Gyorgy Abraham <gyorgy.abraham@protonmail.com>
# Contributor: Pavle Portic <archlinux@theedgeofrage.com>

pkgname=goose
pkgver=3.24.3
pkgrel=1
pkgdesc="Database migration tool written in Go"
arch=(i686 x86_64)
url=https://github.com/pressly/goose
license=(MIT)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/v${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('f0d0e654b7f1e242beb27b49db7b3dbb0788e330c9c4a9e88701c7d3570eaa00')

prepare() {
  cd $pkgname-$pkgver
  export GOPATH="${srcdir}/gopath"
  go mod download
}

check() {
  cd $pkgname-$pkgver
  go test -v -mod=readonly ./...
}

build() {
  cd $pkgname-$pkgver

  export CGO_LDFLAGS="$LDFLAGS"
  export CGO_CFLAGS="$CFLAGS"
  export CGO_CPPFLAGS="$CPPFLAGS"
  export CGO_CXXFLAGS="$CXXFLAGS"

  export GOPATH="${srcdir}/gopath"

  go build \
    -o goose \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    ./cmd/goose
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 goose "${pkgdir}/usr/bin/goose"
  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
