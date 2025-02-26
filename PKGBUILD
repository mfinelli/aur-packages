# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=gnome-shell-extension-emoji-copy
pkgver=26
pkgrel=1
pkgdesc="simplify emoji selection and clipboard management"
arch=(any)
url=https://github.com/FelipeFTN/emoji-copy
license=(GPL-3.0-or-later)
depends=(gnome-shell)
makedepends=(python python-requests zip)
source=(emoji-copy-$pkgver.tar.gz::${url}/archive/v$pkgver.tar.gz)
sha256sums=('8daffcb6380931cd5979e13bc0f2e1726e152afb2ec920877805363a67b911b4')

build() {
  cd Emoji-Copy-$pkgver
  make
}

package() {
  cd Emoji-Copy-$pkgver

  local metadata="$(find . -name 'metadata.json' | grep -v zip-temp)"
  local uuid="$(grep -Po '(?<="uuid": ")[^"]*' "$metadata")"
  local schema=$(grep -Po '(?<="settings-schema": ")[^"]*' \
    "$metadata").gschema.xml
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -dm0755 "$destdir"
  bsdtar xvf ${uuid}.zip -C "$destdir/" --no-same-owner
  install -Dm0644 "$destdir/schemas/$schema" \
    -t "$pkgdir/usr/share/glib-2.0/schemas/"

  # for some reason this doesn't get copied into the zip so just grab it
  # manually
  install -m0644 "${uuid}/data/emojis.db" "$destdir/data/emojis.db"

  rm -rf "${destdir}/schemas"
}

# vim: set ts=2 sw=2 et:
