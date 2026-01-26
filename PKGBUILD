# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=gnome-shell-extension-copyous
pkgver=1.3.0
pkgrel=2
pkgdesc="Modern Clipboard Manager for GNOME"
arch=(any)
url=https://github.com/boerdereinar/copyous
license=(GPL-3.0-or-later)
depends=(gnome-shell)
makedepends=(git nodejs pnpm)
source=($pkgname::git+$url.git#tag=v$pkgver)
sha256sums=('2103144518ed139f7f2937fa8f41514d9dcb862739e2736cf4cd0f71d17a8aac')

prepare() {
  cd $pkgname
  git submodule update --init

  # remove the install script so that it doesn't automatically call
  # "make install"
  sed -i '/"install":/d' package.json

  pnpm install --frozen-lockfile
}

build() {
  cd $pkgname
  make build
}

package() {
  cd $pkgname/dist

  local uuid=$(grep -Po '(?<="uuid": ")[^"]*' metadata.json)
  local schema=$(grep -Po '(?<="settings-schema": ")[^"]*' metadata.json).gschema.xml
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -d "${destdir}"
  bsdtar xvf ${uuid}.zip -C "$pkgdir/usr/share/gnome-shell/extensions/${uuid}/" --no-same-owner
  install -Dm644 "schemas/${schema}" -t "${pkgdir}/usr/share/glib-2.0/schemas/"

  rm -rf "${destdir}/schemas"
}

# vim: set ts=2 sw=2 et:
