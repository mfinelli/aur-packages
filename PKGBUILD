# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Igor Dyatlov <dyatlov.igor@protonmail.com>

pkgname=gnome-shell-extension-randomwallpaper
pkgver=3.1.0
pkgrel=3
pkgdesc="Random Wallpapers for Gnome 3"
arch=(any)
url=https://github.com/ifl0w/RandomWallpaperGnome3
license=(MIT)
depends=(gnome-shell)
makedepends=(blueprint-compiler npm)
source=(RandomWallpaperGnome3-$pkgver.tar.gz::${url}/archive/v$pkgver.tar.gz
  https://patch-diff.githubusercontent.com/raw/ifl0w/RandomWallpaperGnome3/pull/221.patch
  gnome49.patch)
sha256sums=('589b92324c15dab83c9efccd52d65beccc12ef547f05c17c709f19fcd672a636'
            '90a79f9d600bb7853a11a825636ee1575952646bc09bcef395bf6280bb540f42'
            'f99dffff5a4f2026616c9a4d73914682783efe169a98d879f5e42bb32217b527')

prepare() {
  cd RandomWallpaperGnome3-$pkgver
  patch -p1 -N -i "${srcdir}/221.patch"
  patch -p1 -N -i "${srcdir}/gnome49.patch"
  npm ci
}

build() {
  cd RandomWallpaperGnome3-$pkgver

  local uuid="$(grep -Po '(?<="uuid": ")[^"]*' src/metadata.json)"
  local schema=$(grep -Po '(?<="settings-schema": ")[^"]*' \
    src/metadata.json).gschema.xml
  mkdir "$uuid"

  # UI
  blueprint-compiler batch-compile "$uuid/ui" src/ui src/ui/*.blp

  # JS
  npx tsc

  # schemas
  mkdir "$uuid/schemas"
  glib-compile-schemas --targetdir="$uuid/schemas" src/schemas

  # static files
  cp "src/schemas/$schema" "$uuid/schemas"
  cp src/metadata.json "$uuid"
  cp src/stylesheet.css "$uuid"

  # pack into zip
  local extra_source=()
  for file in "$uuid"/*; do
    extra_source+=("--extra-source=$file")
  done

  gnome-extensions pack "${extra_source[@]}" "$uuid"
}

package() {
  cd RandomWallpaperGnome3-$pkgver

  local uuid="$(grep -Po '(?<="uuid": ")[^"]*' src/metadata.json)"
  local schema=$(grep -Po '(?<="settings-schema": ")[^"]*' \
    src/metadata.json).gschema.xml
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -dm0755 "$destdir"
  bsdtar xvf ${uuid}.shell-extension.zip -C "$destdir/" --no-same-owner
  install -Dm0644 "$destdir/schemas/$schema" \
    -t "$pkgdir/usr/share/glib-2.0/schemas/"

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # gnome-extensions pack doesn't seem to include the extra-source options
  # no matter what I try, so we'll just manually add them to the package now...
  cd "$uuid"
  find ui -type f -name '*.ui' -exec install -Dm0644 {} -t "$destdir/ui/" \;

  for s in adapter manager ui; do
    find $s -type f -name '*.js' -exec install -Dm0644 {} -t "$destdir/$s/" \;
  done

  for js in *.js; do
    [[ $js == extension.js ]] && continue
    [[ $js == prefs.js ]] && continue
    install -Dm0644 $js -t "$destdir/"
  done

  rm -rf "${destdir}/schemas"
}

# vim: set ts=2 sw=2 et:
