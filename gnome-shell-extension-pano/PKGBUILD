# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Christian Schendel (doppelhelix@gmail.com)
# Contributor: Kevin Concilio (kevin.concilio1@gmail.com)

pkgname=gnome-shell-extension-pano
pkgver=23alpha5
version=23-alpha5
epoch=1
pkgrel=2
pkgdesc="Next-gen Clipboard Manager for Gnome Shell"
arch=(any)
url=https://github.com/oae/gnome-shell-pano
license=(GPL-2.0-or-later)
depends=(gnome-shell libgda6)
makedepends=(git yarn zip)
source=($pkgname-$pkgver::$url/archive/refs/tags/v$version.tar.gz gnome49.patch)
sha256sums=('03e43d77ed516ff41170b52274a2b79bdda9ca66b809883c6468afa091c6291a'
            'ec255f684ebd2d08cbac7507edca2ffb2c8e1653638f144894c96a3a2c997978')

prepare() {
  cd "gnome-shell-pano-$version"
  patch -p1 -N -i "${srcdir}/gnome49.patch"
  yarn install
}

build() {
  cd "gnome-shell-pano-$version"
  yarn build:package
}

package() {
  cd "gnome-shell-pano-$version/dist"

  local uuid=$(grep -Po '(?<="uuid": ")[^"]*' metadata.json)
  local schema=$(grep -Po '(?<="settings-schema": ")[^"]*' metadata.json).gschema.xml
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -d "${destdir}"
  bsdtar xvf ${uuid}.zip -C "$pkgdir/usr/share/gnome-shell/extensions/${uuid}/"
  install -Dm644 "schemas/${schema}" -t "${pkgdir}/usr/share/glib-2.0/schemas/"

  rm -rf "${destdir}/schemas"
}

# vim: set ts=2 sw=2 et:
