# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=templ
pkgver=0.3.977
pkgrel=1
pkgdesc="A language for writing HTML user interfaces in Go"
arch=(x86_64)
url=https://templ.guide
license=(MIT)
depends=(glibc)
makedepends=(go)
checkdepends=(gopls prettier)
source=(https://github.com/a-h/templ/archive/v${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('fbad3c9e2b6d9b551b1831de30b450d0735f4a9c0773f4f92fb1a16d1243af46')

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
    -o templ \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    ./cmd/templ
}

check() {
  cd $pkgname-$pkgver
  export GOPATH="$srcdir/gopath"
  # go test -mod=readonly ./...
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 templ "$pkgdir/usr/bin/templ"
  install -vDm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
