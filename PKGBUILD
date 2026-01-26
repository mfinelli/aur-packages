# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Oscar Cowdery Lack <oscar.cowderylack@gmail.com>

pkgname=nats-server
pkgver=2.12.3
pkgrel=1
pkgdesc="High-Performance server for NATS.io, the cloud and edge native messaging system"
arch=(x86_64)
url=https://github.com/nats-io/nats-server
license=(Apache-2.0)
depends=(glibc)
makedepends=(git go)
source=("$pkgname::git+$url.git#tag=v$pkgver")
sha256sums=('8c3b24532a66223502ab786ff65c9a8e0b14f83c4a13600a8fdd61f2f775b754')

prepare() {
  cd $pkgname
  export GOPATH="${srcdir}/gopath"
  go mod download
}

check() {
  cd $pkgname
  # TODO: tests try to communicate with real syslog
  # go test -v -mod=readonly ./...
}

build() {
  cd $pkgname

  export CGO_LDFLAGS="$LDFLAGS"
  export CGO_CFLAGS="$CFLAGS"
  export CGO_CPPFLAGS="$CPPFLAGS"
  export CGO_CXXFLAGS="$CXXFLAGS"

  export GOPATH="${srcdir}/gopath"

  go build \
    -o nats-server \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    .
}

package() {
    cd "$pkgname"
    install -Dm0755 nats-server "$pkgdir/usr/bin/$pkgname"
    install -Dm0644 -t "$pkgdir/usr/share/doc/$pkgname/" *.md
    install -Dm0644 -t "$pkgdir/usr/share/doc/$pkgname/" *.txt

    install -Dm0644 -t "$pkgdir/usr/lib/systemd/system" util/*.service
}

# vim: set ts=2 sw=2 et:
