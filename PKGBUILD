# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: pancho horrillo <pancho at pancho dot name>

pkgname=ansible-aur
epoch=1
pkgver=0.12.0
pkgrel=1
pkgdesc="Ansible module to manage packages from the AUR"
arch=(any)
url=https://github.com/kewlfft/ansible-aur
license=(GPL-3.0-or-later)
source=($url/archive/v$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('356095727931d95db48604c4194a1037b1ce946afa1dd27f506e2bebda8a1cf7')

package() {
  cd $pkgname-$pkgver

  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 plugins/modules/aur.py \
    "$pkgdir/usr/share/ansible/plugins/modules/aur.py"
}
