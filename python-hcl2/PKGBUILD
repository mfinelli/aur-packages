# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=python-hcl2
pkgver=8.1.2
pkgrel=1
pkgdesc="A parser for HCL2 written in Python"
arch=(any)
url=https://github.com/amplify-education/python-hcl2
license=(MIT)
depends=(python python-lark python-regex)
makedepends=(git python-build python-installer python-setuptools 
             python-setuptools-scm python-wheel)
checkdepends=(python-coverage python-mock python-nose2)
source=(git+${url}.git#tag=v${pkgver})
sha256sums=('7caa6cfe7f0e5ea1e3f9363f51a3dd62effb02867514da1c8017a7315da55318')

build() {
  cd $pkgname
  python -m build --wheel --no-isolation
}

check() {
  cd $pkgname
  nose2 --config tox.ini --verbose
}

package() {
  cd $pkgname
  python -m installer --destdir="$pkgdir" dist/*.whl

  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
