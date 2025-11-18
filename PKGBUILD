# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: ml <>

pkgname=helm-secrets
pkgver=4.7.0
pkgrel=1
pkgdesc="Helm plugin to manage secrets with Git workflow and store them anywhere"
arch=(any)
url=https://github.com/jkroepke/helm-secrets
license=(Apache-2.0)
install=helm-secrets.install
depends=(bash helm)
optdepends=(
  'sops: secret driver'
  'vault: secret driver'
)
source=(https://github.com/jkroepke/helm-secrets/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('3586ad1d0e9ed35f73d388c4925ba0515a5bd43517e3c84c9fb88505b6db7d8d')

package() {
  cd "${pkgname}-${pkgver}"
  local _dest="${pkgdir}/usr/lib/helm/plugins/${pkgname##helm-}"
  install -Dvm0644 plugin.yaml -t "$_dest"
  cp -avr scripts/ -t "$_dest"

  install -Dvm0644 docs/*.md -t "$pkgdir/usr/share/doc/$pkgname/"

  # NB after the above because there is an empty README.md in the docs/ folder
  install -Dvm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dvm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
