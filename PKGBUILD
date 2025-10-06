# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=gnome-shell-extension-simpleweather
pkgver=49.0.0
pkgrel=1
pkgdesc="A highly configurable GNOME shell extension for viewing the weather"
arch=(any)
url=https://github.com/romanlefler/SimpleWeather
license=(GPL-3.0-or-later)
depends=(gnome-shell)
makedepends=(npm typescript zip)
source=($pkgname-$pkgver.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz)
sha256sums=('953c1db151df60f0d6bb6a1e2fc21a0710df4ae33fd037e5439ea67f479e6b28')

prepare() {
  cd SimpleWeather-$pkgver
  npm ci
}

build() {
  cd SimpleWeather-$pkgver
  make
  make pack
}

package() {
  cd SimpleWeather-$pkgver

  local uuid="$(awk -F\" '/uuid/ {print $4}' static/metadata.json)"
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -dm0755 "$destdir"
  bsdtar xvf dist/simple-weather-v$pkgver.zip -C "$destdir/" --no-same-owner
  install -Dm0644 -t "$pkgdir/usr/share/glib-2.0/schemas/" \
    "$destdir"/schemas/*.xml
  rm -rf "${destdir}/schemas"
}

# vim: set ts=2 sw=2 et:
