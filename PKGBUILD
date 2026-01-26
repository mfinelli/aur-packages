# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=otel-tui
pkgver=0.6.3
pkgrel=1
pkgdesc="A terminal OpenTelemetry viewer inspired by otel-desktop-viewer"
arch=(x86_64)
url=https://github.com/ymtdzzz/otel-tui
license=(Apache-2.0)
depends=(glibc libx11)
makedepends=(go)
source=(${url}/archive/v${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('330be1f04316cb12e24e3fc35cc9b7968e39ae7ab20e1d4380cee0fcc7a0efdb')

prepare() {
  cd $pkgname-$pkgver
  export GOPATH="${srcdir}/gopath"
  go mod download
}

build() {
  cd $pkgname-$pkgver

  export CGO_LDFLAGS="$LDFLAGS"
  export CGO_CFLAGS="$CFLAGS"
  export CGO_CPPFLAGS="$CPPFLAGS"
  export CGO_CXXFLAGS="$CXXFLAGS"

  export GOPATH="${srcdir}/gopath"

  go build \
    -o otel-tui \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    ./...
}

check() {
  cd $pkgname-$pkgver
  export GOPATH="${srcdir}/gopath"
  go test -v -mod=readonly ./...
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 otel-tui "$pkgdir/usr/bin/otel-tui"
}

# vim: set ts=2 sw=2 et:
