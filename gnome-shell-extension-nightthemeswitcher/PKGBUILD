# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Igor Dyatlov <dyatlov.igor@protonmail.com>

pkgname=gnome-shell-extension-nightthemeswitcher
pkgver=80
pkgrel=1
pkgdesc="Automatically toggle your light and dark themes variants"
arch=(any)
url=https://gitlab.com/rmnvgr/nightthemeswitcher-gnome-shell-extension
license=(GPL-3.0-or-later)
depends=(gnome-shell)
makedepends=(git meson npm)
source=($pkgname::git+$url.git#tag=$pkgver)
sha256sums=('6de16b240a640632b6a69b5f084908d5f8edb80a15132dc28b42db5b8ef9a60c')

prepare() {
  cd $pkgname
  npm ci
}

build() {
  cd $pkgname
  arch-meson . build
  meson compile -C build
}

package() {
  cd $pkgname
  meson install -C build --destdir "$pkgdir"
}

# vim: set ts=2 sw=2 et:
