# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: asm0dey <pavel.finkelshtein@gmail.com>

_gemname=pastel
pkgname=ruby-$_gemname
pkgver=0.8.0
pkgrel=2
pkgdesc="Terminal strings styling with intuitive and clean API"
arch=(any)
url=https://github.com/piotrmurach/pastel
license=(MIT)
depends=(ruby ruby-tty-color)
checkdepends=(ruby-bundler ruby-rake ruby-rspec)
makedepends=(ruby-rdoc rubygems)
options=(!emptydirs)
source=(${url}/archive/v${pkgver}/$_gemname-$pkgver.tar.gz
  ${url}/commit/bbadeb031a60650e8c58044d5188be7a2fa669fe.patch)
sha256sums=('9dfeef7d9aa91592aa48efb4644ec8dd70dcb61c0868707c24da9dd6f0d268c3'
            '84892625b7da569f998918449909a4ed4d0bb401648570f16adb5eace5519cf7')

prepare() {
  cd $_gemname-$pkgver
  patch -p1 -N -i "${srcdir}/bbadeb031a60650e8c58044d5188be7a2fa669fe.patch"
}

build() {
  cd $_gemname-$pkgver
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
  cd $_gemname-$pkgver
  local _gemdir="$(gem env gemdir)"
  GEM_HOME="tmp_install/${_gemdir}" rake spec
}

package() {
  cd $_gemname-$pkgver

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -v -Dm0644 LICENSE* "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -v -Dm0644 CHANGELOG.md -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -v -Dm0644 README.md -t "${pkgdir}/usr/share/doc/${pkgname}"
}

# vim: set ts=2 sw=2 et:
