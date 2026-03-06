# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=wgo
pkgver=0.6.3
pkgrel=1
pkgdesc="Live reload for Go apps"
arch=(x86_64)
url=https://github.com/bokwoon95/wgo
license=(MIT)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/v${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('d4f87fe88b314d66977ccb97482cb0acf960e8bd0936f98ab0036cb24f964115')

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
    -o wgo \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    ./...
}

check() {
  cd $pkgname-$pkgver
  export GOPATH="$srcdir/gopath"
  go test -mod=readonly -v ./...
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 wgo "$pkgdir/usr/bin/wgo"
  install -vDm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
