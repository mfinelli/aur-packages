# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Andrea Richiardi <a.richiardi.work@gmail.com>
# Contributor: Andreas 'Segaja' Schleifer <archlinux at segaja dot de>
# Contributor: Jeremy Audet <jerebear@protonmail.com>
# Contributor: Julien Nicoulaud <julien.nicoulaud@gmail.com>

_gemname=ruby_parser
pkgname=ruby-${_gemname}
pkgver=3.21.1
pkgrel=1
pkgdesc="Ruby parser written in pure ruby"
arch=(any)
url=https://github.com/seattlerb/ruby_parser
license=(MIT)
options=(!emptydirs)
depends=(ruby ruby-racc ruby-sexp_processor)
makedepends=(ruby-hoe ruby-minitest ruby-oedipus_lex ruby-rake unifdef)
source=(${url}/archive/v${pkgver}/$_gemname-$pkgver.tar.gz)
sha256sums=('e00221b31030a382a851999faf906b98f3b5c7917879b01ab1983f31e6d399ee')

prepare() {
  cd "${_gemname}-${pkgver}"

  # update gemspec/Gemfile to allow newer version of the dependencies
  sed --in-place --regexp-extended 's|~>|>=|g' Rakefile

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
  install -v -Dm0644 *.md -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -v -Dm0644 *.rdoc -t "${pkgdir}/usr/share/doc/${pkgname}"
}

# vim: set ts=2 sw=2 et:
