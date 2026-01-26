# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Igor Dyatlov <dyatlov.igor@protonmail.com>

pkgname=gnome-shell-extension-nightthemeswitcher
pkgver=81
pkgrel=1
pkgdesc="Automatically toggle your light and dark themes variants"
arch=(any)
url=https://gitlab.com/rmnvgr/nightthemeswitcher-gnome-shell-extension
license=(GPL-3.0-or-later)
depends=(gnome-shell)
makedepends=(git meson npm)
source=($pkgname::git+$url.git#tag=$pkgver)
sha256sums=('a47c0b67d2febfc947c612ad2691f86e89f5e03d8f5ac5808ca5ec8573183e4a')

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
