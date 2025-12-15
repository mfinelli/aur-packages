# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Andrea Richiardi <a.richiardi.work@gmail.com>
# Contributor: Jeremy Audet <jerebear@protonmail.com>
# Contributor: Julien Nicoulaud <julien.nicoulaud@gmail.com>

_gemname=sexp_processor
pkgname=ruby-${_gemname}
pkgver=4.17.4
pkgrel=1
pkgdesc="A branch of ParseTree that brings several sexp processing tools"
arch=(any)
url=https://github.com/seattlerb/sexp_processor
license=(MIT)
depends=(ruby)
checkdepends=(ruby-minitest)
makedepends=(ruby-hoe ruby-rake ruby-rdoc rubygems)
options=(!emptydirs)
source=(${url}/archive/v${pkgver}/$_gemname-$pkgver.tar.gz)
sha256sums=('986a8e49a611715d92bc3ea844dadabdb06b45424989e7e6b1e0eb971e7d86fa')

prepare() {
  cd $_gemname-$pkgver

  # there is no license file in the repo, but we can extract one
  sed '/== LICENSE:/,$!d' README.rdoc > LICENSE
}

build() {
  cd $_gemname-$pkgver
  local _gemdir="$(gem env gemdir)"

  rake gem

  gem install \
    --local \
    --verbose \
    --ignore-dependencies \
    --no-user-install \
    --install-dir "tmp_install/${_gemdir}" \
    --bindir "tmp_install/usr/bin" \
    "pkg/${_gemname}-${pkgver}.gem"

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
  GEM_HOME="tmp_install/${_gemdir}" rake test
}

package() {
  cd $_gemname-$pkgver

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -v -Dm0644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -v -Dm0644 *.rdoc -t "${pkgdir}/usr/share/doc/${pkgname}"
}

# vim: set ts=2 sw=2 et:
