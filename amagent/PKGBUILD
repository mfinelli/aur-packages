# Maintainer: Mario Finelli <mario at finel dot li>

# Reverse engineered from:
# wget 'https://console.automox.com/downloadInstaller?accesskey=MYKEY' \
#   -O automox-installer.sh

pkgname=amagent
pkgver=2.3.34
pkgrel=1
arch=("x86_64")
pkgdesc="Automox agent"
url=https://www.automox.com
license=(custom)
depends=(curl dmidecode lsb-release lshw)
install=amagent.install
source=(file://${pkgname}_${pkgver}_amd64.deb)
sha256sums=('f6cf9018127e5bfa1a7c278dbc04a92d64dfee2e85e4776d1bb6a06dd835c976')

prepare() {
  mkdir "$srcdir/$pkgname"
  bsdtar xf "$srcdir/data.tar.xz" -C "$srcdir/$pkgname"
}

package() {
  install -Dm0644 "$srcdir/$pkgname/usr/share/doc/amagent/copyright" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  install -Dm0644 "$srcdir/$pkgname/lib/systemd/system/amagent.service" \
    "$pkgdir/usr/lib/systemd/system/amagent.service"

  install -dm0750 "$pkgdir/var/lib/amagent"

  for bin in amagent agent-db-migration osqueryi; do
    install -Dm0755 "$srcdir/$pkgname/opt/amagent/$bin" \
      "$pkgdir/opt/amagent/$bin"
  done
}

# vim: set ts=2 sw=2 et:
