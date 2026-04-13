# Maintainer: Mario Finelli <mario at finel dot li>

# project doesn't tag releases, so we need to grab the commits from when the
# metadata.json version field changes
# see: https://github.com/icedman/search-light/issues/137
_commit=4e93e0e3e2fba8512dfd588177b7a6a2a71c9f1e

pkgname=gnome-shell-extension-search-light
pkgver=101
pkgrel=1
pkgdesc="Gnome Extension to bring macOS's Spotlight functionality"
arch=(any)
url=https://github.com/icedman/search-light
license=(GPL-3.0-or-later)
depends=(gnome-shell)
makedepends=(git zip)
source=(git+${url}.git#commit=$_commit)
sha256sums=('07b76fb3b86eca7e8b80e05e3a4a451f0dda24e357b6e82e1fb0a575aaa5a505')

build() {
  cd search-light
  make build
  make publish
}

package () {
  cd search-light

  local uuid=$(grep -Po '(?<="uuid": ")[^"]*' metadata.json)
  local schema=$(grep -Po '(?<="schema-id": ")[^"]*' metadata.json).gschema.xml
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -d "${destdir}"
  bsdtar xvf ${uuid}.zip -C "$pkgdir/usr/share/gnome-shell/extensions/${uuid}/" --no-same-owner
  install -Dm0644 "schemas/${schema}" -t "${pkgdir}/usr/share/glib-2.0/schemas/"

  rm -rf "${destdir}/schemas"
}

# vim: set ts=2 sw=2 et:
