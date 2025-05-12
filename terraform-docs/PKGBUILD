# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Jeff Henson <jeff@henson.io>
# Contributor: Gabriel M. Dutra <0xdutra@gmail.com>

pkgname=terraform-docs
pkgver=0.20.0
pkgrel=1
pkgdesc="Generate documentation from Terraform modules in various output formats"
arch=(x86_64 i686 armv7h aarch64)
url="https://github.com/terraform-docs/terraform-docs"
license=(MIT)
depends=(glibc)
makedepends=(go)
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/v${pkgver}.tar.gz")
sha256sums=('793ad60be207292b9f27664d5c73bd75512e7a5e458b0fe2daa872b5ad46d6a9')

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
    -o $pkgname \
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
  install -vDm0755 $pkgname "$pkgdir/usr/bin/$pkgname"
  install -vDm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -vDm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"

  "${pkgdir}/usr/bin/${pkgname}" completion bash | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/bash-completion/completions/${pkgname}"
  "${pkgdir}/usr/bin/${pkgname}" completion zsh | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/zsh/site-functions/_${pkgname}"
  "${pkgdir}/usr/bin/${pkgname}" completion fish | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/fish/vendor_completions.d/${pkgname}.fish"
}

# vim: set ts=2 sw=2 et:
