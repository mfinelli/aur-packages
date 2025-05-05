# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=gimp-fourier
pkgver=0.4.5gimp3rc1
pkgrel=1
pkgdesc="A GIMP plugin to do the fourier transform"
arch=(i686 x86_64)
license=(GPL-3.0-or-later)
url="https://www.lprp.fr/gimp_plugin_en/#fourier"
depends=(fftw glibc gimp)
source=(https://github.com/rpeyron/plugin-gimp-fourier/archive/refs/tags/v0.4.5+gimp3rc1.tar.gz)
sha256sums=('8406fa49b8d31444b5870063eaf0dbd89b6e909231b3fa57d083c05fd38c134a')

prepare() {
  cd plugin-gimp-fourier-0.4.5-gimp3rc1
  autoreconf -i
}

build() {
  cd plugin-gimp-fourier-0.4.5-gimp3rc1
  ./configure --enable-gimp3-fourier
  make
}

package() {
  cd plugin-gimp-fourier-0.4.5-gimp3rc1
  make install DESTDIR="$pkgdir" PREFIX=/usr
}

# vim: set ts=2 sw=2 et:
