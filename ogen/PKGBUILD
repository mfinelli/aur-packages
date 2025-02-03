# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=ogen
pkgver=1.9.0
pkgrel=1
pkgdesc="OpenAPI v3 code generator for go"
arch=(x86_64)
url=https://ogen.dev
license=(Apache-2.0)
depends=(glibc)
makedepends=(go)
source=("$pkgname-$pkgver.tar.gz::https://github.com/ogen-go/ogen/archive/v$pkgver.tar.gz")
sha256sums=('7f3f75608290e96c6725bb8cdd75cb76499c263804069b932adf1c8eaee4188b')

prepare() {
  cd $pkgname-$pkgver
  export GOPATH="$srcdir/gopath"
  go mod download
}

check() {
  cd $pkgname-$pkgver
  export GOPATH="$srcdir/gopath"
  go test -mod=readonly -v ./...
}

build() {
  cd $pkgname-$pkgver

  export GOPATH="$srcdir/gopath"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"

  go build -o ogen \
    -buildmode=pie \
    -trimpath \
    -mod=readonly \
    -modcacherw \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    cmd/ogen/main.go
}

package() {
  cd $pkgname-$pkgver

  install -Dm0755 $pkgname "$pkgdir/usr/bin/$pkgname"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
