# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=modctl-git
pkgver=r224.66753dd
pkgrel=1
pkgdesc="command line mod manager"
arch=(aarch64 x86_64)
url=https://modctl.org
license=(GPL-3.0-or-later)
provides=(modctl)
conflicts=(modctl)
depends=(glibc libarchive)
makedepends=(git go sqlc scdoc)
source=(git+https://github.com/mfinelli/modctl.git?signed)
sha256sums=('SKIP')

pkgver() {
  cd modctl
  printf "r%s.%s" "$(git rev-list --count HEAD)" \
    "$(git rev-parse --short HEAD)"
}

prepare() {
  cd modctl
  export GOPATH="${srcdir}/gopath"
  go mod download
}

check() {
  cd modctl
  export GOPATH="${srcdir}/gopath"
  go test -mod=readonly -v ./...
}

build() {
  cd modctl

  export GOPATH="${srcdir}/gopath"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"

  sqlc generate
  make sample.tar.gz
  make modctl.1

  go build -o modctl \
    -buildmode=pie \
    -trimpath \
    -mod=readonly \
    -modcacherw \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    -tags='no_clickhouse no_libsql no_mssql no_mysql no_postgres \
      no_vertica no_ydb' \
    main.go
}

package() {
  cd modctl

  install -vDm0755 modctl "$pkgdir/usr/bin/modctl"

  install -vDm0644 modctl.1 "$pkgdir/usr/share/man/man1/modctl.1"
  ./modctl completion bash | install -vDm0644 /dev/stdin \
    "${pkgdir}/usr/share/bash-completion/completions/modctl"
  ./modctl completion fish | install -vDm0644 /dev/stdin \
    "${pkgdir}/usr/share/fish/vendor_completions.d/modctl.fish"
  ./modctl completion zsh | install -vDm0644 /dev/stdin \
    "${pkgdir}/usr/share/zsh/site-functions/_modctl"

  install -vDm0644 README.md "$pkgdir/usr/share/doc/modctl/README.md"
  install -vDm0644 CHANGELOG.md "$pkgdir/usr/share/doc/modctl/CHANGELOG.md"
  ./pkg/copydocs.bash "$pkgdir/usr/share/doc/modctl"
}

# vim: set ts=2 sw=2 et:
