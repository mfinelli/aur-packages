# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: ml <>

pkgname=helm-secrets
pkgver=4.6.11
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
sha256sums=('fe72a5fdf9eb8a2864a8e9d0b44f10df7b4174c5f9294438c6af0b023315b4ff')

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
