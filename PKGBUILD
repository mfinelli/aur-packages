# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=dockerfmt
pkgver=0.2.6
pkgrel=1
pkgdesc="Dockerfile formatter - a modern dockfmt"
arch=(x86_64)
url=https://github.com/reteps/dockerfmt
license=(MIT)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('6965c27d38203d56a7e68ef32213e8eefdaed1cf704196fcb09d9f2ea6db1675')

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
    -o dockerfmt \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    main.go
}

check() {
  cd $pkgname-$pkgver
  export GOPATH="${srcdir}/gopath"
  go test -v ./...
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 dockerfmt "$pkgdir/usr/bin/dockerfmt"
  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  "${pkgdir}/usr/bin/dockerfmt" completion bash | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/bash-completion/completions/dockerfmt"
  "${pkgdir}/usr/bin/dockerfmt" completion zsh | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/zsh/site-functions/_dockerfmt"
  "${pkgdir}/usr/bin/dockerfmt" completion fish | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/fish/vendor_completions.d/dockerfmt.fish"
}

# vim: set ts=2 sw=2 et:
