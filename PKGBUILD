# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=newrelic-cli
pkgver=0.96.3
pkgrel=1
pkgdesc="The New Relic Command Line Interface"
arch=(x86_64)
url=https://github.com/newrelic/newrelic-cli
license=(Apache-2.0)
depends=(glibc)
makedepends=(go)
source=(${url}/archive/v${pkgver}/$pkgname-$pkgver.tar.gz)
sha256sums=('149aa40ab6da7b662d3462d7fcf3693f19c1f28a3d3fb57b09adb1cb4a4d7c9a')

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
    -o bin/newrelic \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\" \
      -X main.appName=newrelic-cli \
      -X github.com/newrelic/newrelic-vli/internal/cli.version=${pkgver}" \
    cmd/newrelic/*
}

package() {
  cd $pkgname-$pkgver
  install -Dm0755 bin/newrelic "$pkgdir/usr/bin/newrelic"

  "${pkgdir}/usr/bin/newrelic" completion --shell bash | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/bash-completion/completions/newrelic"
  "${pkgdir}/usr/bin/newrelic" completion --shell zsh | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/zsh/site-functions/_newrelic"
  "${pkgdir}/usr/bin/newrelic" completion --shell fish | install -Dm0644 \
    /dev/stdin "${pkgdir}/usr/share/fish/vendor_completions.d/newrelic.fish"
}

# vim: set ts=2 sw=2 et:
