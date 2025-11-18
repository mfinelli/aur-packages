# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=gnome-shell-extension-veil
pkgver=0.3.0
pkgrel=1
pkgdesc="A cleaner, quieter GNOME panel"
arch=(any)
url=https://github.com/dagimg-dot/veil
license=(MIT)
depends=(gnome-shell)
makedepends=(bun zip)
source=($pkgname-$pkgver.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz)
sha256sums=('52207a0ac92582f99fae50894f312642f18d9dc09310d39cca47be7d18fb61e8')

prepare() {
  cd veil-$pkgver
  bun install
}

build() {
  cd veil-$pkgver
  bun run build
}

package() {
  cd veil-$pkgver

  local uuid="$(awk -F\" '/uuid/ {print $4}' metadata.json)"
  local destdir="${pkgdir}/usr/share/gnome-shell/extensions/${uuid}"

  install -dm0755 "$destdir"
  bsdtar xvf build/$uuid.shell-extension-v$pkgver.zip -C "$destdir/" \
    --no-same-owner
  install -Dm0644 -t "$pkgdir/usr/share/glib-2.0/schemas/" \
    "$destdir"/schemas/*.xml
  rm -rf "${destdir}/schemas"

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
