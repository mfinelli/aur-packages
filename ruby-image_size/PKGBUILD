# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Andrew Rabert <draje@nullsum.net>

_gemname=image_size
pkgname=ruby-${_gemname}
pkgver=3.6.0
pkgrel=1
pkgdesc="measure image size using pure Ruby"
arch=(any)
url=https://github.com/toy/image_size
license=(Ruby GPL-2.0-or-later)
options=(!emptydirs)
depends=(ruby)
checkdepends=(ruby-rspec ruby-rubocop ruby-rubocop-rspec ruby-webrick)
makedepends=(rubygems ruby-rdoc)
source=(${url}/archive/v$pkgver/$_gemname-$pkgver.tar.gz)
sha256sums=('9ab0066fbf3b6d2dc239f8d0dfb7a55ccb24fe79c4a5f19032c5d839cab7603b')

build() {
  cd ${_gemname}-${pkgver}
  local _gemdir="$(gem env gemdir)"

  gem build "${_gemname}.gemspec"

  gem install \
    --local \
    --verbose \
    --ignore-dependencies \
    --no-user-install \
    --install-dir "tmp_install/${_gemdir}" \
    --bindir "tmp_install/usr/bin" \
    "${_gemname}-${pkgver}.gem"

  # remove unrepreducible files
  rm --force --recursive --verbose \
    "tmp_install/${_gemdir}/cache/" \
    "tmp_install/${_gemdir}/gems/${_gemname}-${pkgver}/vendor/" \
    "tmp_install/${_gemdir}/doc/${_gemname}-${pkgver}/ri/ext/"

  find "tmp_install/${_gemdir}/gems/" \
    -type f \
    \( \
      -iname "*.o" -o \
      -iname "*.c" -o \
      -iname "*.so" -o \
      -iname "*.time" -o \
      -iname "gem.build_complete" -o \
      -iname "Makefile" \
    \) \
    -delete

  find "tmp_install/${_gemdir}/extensions/" \
    -type f \
    \( \
      -iname "mkmf.log" -o \
      -iname "gem_make.out" \
    \) \
    -delete
}

check() {
  cd ${_gemname}-${pkgver}
  local _gemdir="$(gem env gemdir)"
  GEM_HOME="tmp_install/${_gemdir}" rspec
}

package() {
  cd ${_gemname}-${pkgver}

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 README.markdown "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.markdown \
    "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
