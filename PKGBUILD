# Maintainer: Gyorgy Abraham <gyorgy.abraham@protonmail.com>
# Maintainer: Pavle Portic <archlinux@theedgeofrage.com>

pkgname=goose
pkgver=3.16.0
pkgrel=1
pkgdesc='Database migration tool written in Go.'
arch=('i686' 'x86_64')
url='https://github.com/pressly/goose'
license=('MIT')
depends=('go')
makedepends=('git' 'go')
source=("https://github.com/pressly/$pkgname/archive/v$pkgver.tar.gz")
sha256sums=('de4d1579a28e6205282fd2eec849fb83a133b579435c2726343bdeb145f5f25e')

build() {
	cd $pkgname-$pkgver
	go build -o goose ./cmd/goose
}

package() {
	cd $pkgname-$pkgver
	install -Dm755 "goose" "${pkgdir}/usr/bin/goose"
}
