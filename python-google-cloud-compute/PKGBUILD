# Maintainer: Mario Finelli <mario at finel dot li>

_name=google-cloud-compute
pkgname=python-$_name
pkgver=1.43.0
pkgrel=1
pkgdesc="Google Cloud Compute python SDK"
url=https://github.com/googleapis/google-cloud-python
arch=(any)
license=(Apache-2.0)
depends=(python python-google-api-core python-proto-plus python-protobuf
         python-google-auth python-grpcio)
makedepends=(python-setuptools python-wheel python-installer python-build)
options=(!emptydirs)
source=("https://files.pythonhosted.org/packages/source/${_name::1}/${_name//-/_}/${_name//-/_}-$pkgver.tar.gz")
sha256sums=('113fd6a5e1832e5d032b94e391c3bd7e3b06e1bee0ed6665085dfefffb41360a')

build() {
  cd "${_name//-/_}-$pkgver"
  python -m build --wheel --no-isolation
}

package() {
  cd "${_name//-/_}-$pkgver"
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dm644 README.rst "$pkgdir/usr/share/doc/$pkgname/README.rst"
}

# vim: set ts=2 sw=2 et:
