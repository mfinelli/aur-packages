# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=redli
pkgver=0.15.0
pkgrel=1
pkgdesc="A humane alternative to the Redis-cli and TLS"
arch=(x86_64)
url=https://github.com/IBM-Cloud/redli
license=(Apache-2.0)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/v${pkgver}/${pkgname}-v${pkgver}.tar.gz)
sha256sums=('12ae9937e0da122f8566e3d0d71aa8ea9756e6a3e4382589b7395b199c076839')

prepare() {
  cd redli-$pkgver
  export GOPATH="${srcdir}/gopath"
  go mod download
}

check() {
  cd redli-$pkgver
  go test ./...
}

build() {
  cd redli-$pkgver
  export GOPATH="${srcdir}/gopath"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"
  go build -o redli
}

package() {
  cd redli-$pkgver
  install -Dm0755 redli "$pkgdir/usr/bin/redli"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}
