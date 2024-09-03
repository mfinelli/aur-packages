# Maintainer: Oscar Cowdery Lack <oscar.cowderylack@gmail.com>
pkgname=nats-server
pkgver=2.10.20
pkgrel=1
pkgdesc="High-Performance server for NATS.io, the cloud and edge native messaging system"
arch=(x86_64)
url="https://github.com/nats-io/nats-server"
license=('Apache-2.0')
makedepends=(git go)
source=("$pkgname::git+$url#tag=v$pkgver")
sha256sums=('a38a62f64c000a7857e1f46853e842b5baffb7b530558516886cccf2b275281d')

build() {
    cd "$pkgname"
    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"
    export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"

    go build .
}

package() {
    cd "$pkgname"
    install -Dm755 ./$pkgname "$pkgdir"/usr/bin/$pkgname
}
