# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=otel-tui
pkgver=0.3.10
pkgrel=1
pkgdesc="A terminal OpenTelemetry viewer inspired by otel-desktop-viewer"
arch=(x86_64)
url=https://github.com/ymtdzzz/otel-tui
license=(Apache-2.0)
depends=(glibc libx11)
makedepends=(go)
source=(${url}/archive/v${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('f3ba9f51c5a4c28201d0a7fab1a53f57bbf19c23b589a4c5fd74ff35738ffec8')

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

package() {
  cd $pkgname-$pkgver
  install -Dm0755 otel-tui "$pkgdir/usr/bin/otel-tui"
}

# vim: set ts=2 sw=2 et:
