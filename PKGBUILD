# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Jeremy Audet <jerebear@protonmail.com>
# Contributor: Julien Nicoulaud <julien.nicoulaud@gmail.com>

_gemname=ruby2ruby
pkgname=ruby-$_gemname
pkgver=2.5.0
pkgrel=1
pkgdesc="Provides a means of generating pure ruby code easily"
arch=(any)
url=https://github.com/seattlerb/ruby2ruby
license=(MIT)
options=(!emptydirs)
depends=(ruby ruby-ruby_parser ruby-sexp_processor)
makedepends=(ruby-hoe ruby-rdoc rubygems)
source=(${url}/archive/v${pkgver}/$_gemname-$pkgver.tar.gz
       https://github.com/seattlerb/ruby2ruby/commit/94c39959019f93ce2a8346060bc4d4c109ead1ac.patch
       https://github.com/seattlerb/ruby2ruby/commit/e935bfdb76419f0a3ba55aac05536132f1e2986d.patch)
sha256sums=('5fdda6031edd4157481a6bf080bbc408ddc0791caff72c37f108fbb17e42614a'
            'fc3ee9229a2239705b4485578601ddb68d5e1ac0f348810a879f3ecbf01f5f25'
            '4524101e5e2161132185f30cd3f1cf9ee7295dd5dfb0cda466a24cde2de1ad82')

prepare() {
  cd $_gemname-$pkgver

  patch -p1 -N -i "${srcdir}/94c39959019f93ce2a8346060bc4d4c109ead1ac.patch"
  patch -p1 -N -i "${srcdir}/e935bfdb76419f0a3ba55aac05536132f1e2986d.patch"

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
