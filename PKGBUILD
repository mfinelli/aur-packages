# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=go-snitch
pkgver=0.2.0
pkgrel=1
pkgdesc="A prettier way to inspect network connections"
arch=(x86_64)
url=https://github.com/karol-broda/snitch
license=(MIT)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/v${pkgver}/snitch-$pkgver.tar.gz)
sha256sums=('247f8fbb13a6739ffbb85db7e4bb5b8110a01e09eb21879cfe543b7e3e6ed6ce')

prepare() {
  cd snitch-$pkgver
  export GOPATH="${srcdir}/gopath"
  go mod download
}

build() {
  cd snitch-$pkgver

  export CGO_LDFLAGS="$LDFLAGS"
  export CGO_CFLAGS="$CFLAGS"
  export CGO_CPPFLAGS="$CPPFLAGS"
  export CGO_CXXFLAGS="$CXXFLAGS"

  export GOPATH="${srcdir}/gopath"

  go build \
    -o snitch \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" .
}

check() {
  cd snitch-$pkgver
  export GOPATH="${srcdir}/gopath"
  go test -v ./...
}

package() {
  cd snitch-$pkgver
  install -Dm0755 snitch "$pkgdir/usr/bin/snitch"
  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  "$pkgdir/usr/bin/snitch" completion bash | install -Dm0644 \
    /dev/stdin "$pkgdir/usr/share/bash-completion/completions/snitch"
  "$pkgdir/usr/bin/snitch" completion zsh | install -Dm0644 \
    /dev/stdin "$pkgdir/usr/share/zsh/site-functions/_snitch"
  "$pkgdir/usr/bin/snitch" completion fish | install -Dm0644 \
    /dev/stdin "$pkgdir/usr/share/fish/vendor_completions.d/snitch.fish"
}

# vim: set ts=2 sw=2 et:
