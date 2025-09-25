# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: George Rawlinson <grawlinson@archlinux.org>

pkgname=go-critic
pkgver=0.13.0
pkgrel=1
pkgdesc="The most opinionated Go source code linter for code audit"
arch=(x86_64)
url=https://go-critic.com
license=(MIT)
depends=(glibc)
makedepends=(git go)
options=(!lto)
source=($pkgname::git+https://github.com/go-critic/go-critic.git#tag=v$pkgver)
sha256sums=('230b5448dd2774d9c4ec95ad0f6cb44b4bf9b9fe30ed3d999b3a6aa887c070d8')

prepare() {
  cd $pkgname
  export GOPATH="${srcdir}/gopath"
  go mod download
}

build() {
  cd $pkgname

  export CGO_LDFLAGS="$LDFLAGS"
  export CGO_CFLAGS="$CFLAGS"
  export CGO_CPPFLAGS="$CPPFLAGS"
  export CGO_CXXFLAGS="$CXXFLAGS"

  export GOPATH="${srcdir}/gopath"

  for bin in gocritic gocritic-analysis; do
    go build -v \
      -o build/ \
      -trimpath \
      -buildmode=pie \
      -mod=readonly \
      -modcacherw \
      -ldflags "-linkmode external -extldflags \"${LDFLAGS}\" \
        -X main.Version=$pkgver" \
      ./cmd/$bin/...
  done
}

check() {
  cd $pkgname
  export GOPATH="$srcdir/gopath"
  go test -mod=readonly -v ./...
}

package() {
  cd $pkgname

  install -vDm0755 -t "$pkgdir/usr/bin" build/*
  install -vDm0644 -t "$pkgdir/usr/share/doc/$pkgname" README.md
  install -vDm0644 -t "$pkgdir/usr/share/licenses/$pkgname" LICENSE
}

# vim: set ts=2 sw=2 et:
