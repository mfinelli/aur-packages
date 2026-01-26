# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: pancho horrillo <pancho at pancho dot name>

pkgname=ansible-aur
epoch=1
pkgver=0.13.1
pkgrel=1
pkgdesc="Ansible module to manage packages from the AUR"
arch=(any)
url=https://github.com/kewlfft/ansible-aur
license=(GPL-3.0-or-later)
source=($url/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('30dfb28abaa29140d38571fa3d8004823066a9d5606a4c5d44af0d63345cb8ae')

package() {
  cd $pkgname-$pkgver

  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 plugins/modules/aur.py \
    "$pkgdir/usr/share/ansible/plugins/modules/aur.py"
}
