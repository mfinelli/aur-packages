# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: ml <>

pkgname=helm-secrets
pkgver=4.6.1
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
sha256sums=('b498632f62f5aa5b059841a23e5e98a5c86ec0aefe07fe919632f7f0bd89330b')

prepare() {
  cd "${pkgname}-${pkgver}"
  sed -i '/platformCommand:/,+2 d' plugin.yaml
}

package() {
  cd "${pkgname}-${pkgver}"
  local _dest="${pkgdir}/usr/lib/helm/plugins/${pkgname##helm-}"
  install -Dm0644 plugin.yaml -t "$_dest"
  # copy whole scripts directory but remove the install script
  cp -ar scripts/ -t "$_dest"
  rm -f "$_dest/install.sh"
}

# vim: set ts=2 sw=2 et:
