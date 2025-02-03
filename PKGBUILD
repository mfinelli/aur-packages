# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=cui
pkgver=0.5.1
pkgrel=1
pkgdesc="http request/response tui"
arch=(x86_64)
url=https://github.com/mfinelli/cui
license=(GPL-3.0-or-later)
depends=(glibc)
makedepends=(go)
source=(${url}/releases/download/v${pkgver}/${pkgname}_v${pkgver}.tar.gz{,.asc})
validpgpkeys=(7A701FCB0E832A8CDADADA907C3ACA9DD0C33A05)
sha256sums=('92fe6665ca1932964cd02eaff1535fc68a21a79450525416378057bc9ca3a362'
            'SKIP')

check() {
  cd ${pkgname}_v${pkgver}
  make test
}

build() {
  cd ${pkgname}_v${pkgver}

  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"

  go build \
    -buildmode=pie \
    -trimpath \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    -mod=vendor \
    -modcacherw \
    -o $pkgname
}

package() {
  cd ${pkgname}_v${pkgver}
  make install DESTDIR="$pkgdir" PREFIX=/usr
}

# vim: set ts=2 sw=2 et:
