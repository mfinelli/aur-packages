# Maintainer: Mario Finelli <mario at finel dot li>

_name=onepasswordconnectsdk
pkgname=python-${_name}
pkgver=1.5.1
pkgrel=1
pkgdesc="Python SDK for 1Password Connect"
arch=(any)
url=https://github.com/1Password/connect-sdk-python
license=(MIT)
depends=(python python-dateutil python-requests python-six)
makedepends=(python-build python-httpx python-installer python-pbs-installer
             python-poetry python-poetry-plugin-export python-trove-classifiers
             python-wheel python-zstandard)
checkdepends=(python-pytest python-pytest-asyncio python-pytest-cov
              python-respx)
# source=(https://files.pythonhosted.org/packages/source/${_name::1}/$_name/$_name-$pkgver.tar.gz)
# we use the tarball from github because the pypi source doesn't include tests
source=(${url}/archive/v$pkgver/$_name-$pkgver.tar.gz)
sha256sums=('5ddf509d6adba46f20967c293f2f298064b5401c25a4230d3fe9c9221cbad38d')

build() {
  cd connect-sdk-python-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  cd connect-sdk-python-$pkgver
  pytest
}

package() {
  cd connect-sdk-python-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl

  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
  install -Dm0644 LICENSE.md "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
