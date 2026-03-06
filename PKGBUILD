# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=time_warrior
pkgver=1.4.2
pkgrel=1
pkgdesc="A CLI-based time tracking tool"
arch=(x86_64)
url=https://github.com/mrcook/time_warrior
license=(MIT)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('3d8c049a43d7d4fc6d23a7fdd31c39f7e0f1d5a51977e85839a15963ec136f8b')

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
    -o tw/tw \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    tw/main.go
}

check() {
  cd $pkgname-$pkgver
  export GOPATH="${srcdir}/gopath"
  # go test -mod=readonly -v ./...
}

package() {
  cd $pkgname-$pkgver
  install -vDm0755 tw/tw "$pkgdir/usr/bin/tw"
  install -vDm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -vDm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
