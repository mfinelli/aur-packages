# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=newrelic-cli
pkgver=0.18.7
pkgrel=1
pkgdesc="The New Relic Command Line Interface"
arch=(x86_64)
url=https://github.com/newrelic/newrelic-cli
license=(Apache)
depends=(glibc)
makedepends=(go git)
source=(git+https://github.com/newrelic/${pkgname}.git?tag=v${pkgver})
sha256sums=('SKIP')

prepare() {
  cd newrelic-cli
  export GOPATH="${srcdir}/gopath"
  go mod vendor
}

build() {
  cd newrelic-cli

  export GOPATH="${srcdir}/gopath"

  go build \
    -o bin/newrelic \
    -trimpath \
    -buildmode=pie \
    -mod=readonly \
    -ldflags "-s -w -linkmode external -extldflags \"${LDFLAGS}\" -X main.appName=newrelic-cli -X main.version=v${pkgver} -X github.com/newrelic/newrelic-cli/internal/client.version=v${pkgver}" \
    cmd/newrelic/*
}

package() {
  cd newrelic-cli
  install -Dm0755 bin/newrelic "$pkgdir/usr/bin/newrelic"

  install -dm0755 "$pkgdir/usr/share/bash-completion/completions" \
    "$pkgdir/usr/share/zsh/site-functions"
  "${pkgdir}/usr/bin/newrelic" completion --shell bash > \
    "${pkgdir}/usr/share/bash-completion/completions/newrelic"
  "${pkgdir}/usr/bin/newrelic" completion --shell bash > \
    "${pkgdir}/usr/share/zsh/site-functions/_newrelic"
}

# vim: set ts=2 sw=2 et:
