# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=flay
pkgname=ruby-$_gemname
pkgver=2.14.0
pkgrel=1
pkgdesc="Analyzes code for structural similarities"
arch=(any)
url=https://github.com/seattlerb/flay
license=(MIT)
options=(!emptydirs)
depends=(ruby ruby-erubi ruby-path_expander ruby-ruby2ruby ruby-ruby_parser
         ruby-sexp_processor)
checkdepends=(ruby-minitest ruby-racc)
makedepends=(ruby-hoe ruby-rake ruby-rdoc rubygems)
source=(${url}/archive/v${pkgver}/$_gemname-$pkgver.tar.gz)
sha256sums=('47b16d4c21de689c4fcefadf647f6115e1d0be177500671d45ec9eee922c4f06')

prepare() {
  cd $_gemname-$pkgver

  # there is no license file in the repo, but we can extract one
  sed '/== License:/,$!d' README.rdoc > LICENSE
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
