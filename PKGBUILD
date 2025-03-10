# Maintainer: Mario Finelli <mario at finel dot li>

# 1.0.6 doesn't have a tag on github, but I think it's this commit...
_commit=6201dd6150a21b047dada6d14997a9d91444a998
_gemname=rspec-block_is_expected
pkgname=ruby-${_gemname}
pkgver=1.0.6
pkgrel=1
pkgdesc="Simplify testing of blocks in RSpec"
arch=(any)
url=https://github.com/pboling/rspec-block_is_expected
license=(MIT)
depends=(ruby)
checkdepends=(ruby-bundler ruby-byebug ruby-rake ruby-rspec
              ruby-rspec-pending_for)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(${url}/archive/$_commit/$_gemname-$pkgver.tar.gz)
sha256sums=('69e21aa3a6fb0e193fb6376a822c8ac23ed4edcdd9c275b19d1ef97e665c7749')

prepare() {
  cd $_gemname-$_commit
  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  sed -i 's|~>|>=|g' Gemfile

  rm Gemfile.lock
  sed -i '/appraisal/d' ${_gemname}.gemspec
  sed -i '/kettle-soup-cover/d' Gemfile
  sed -i '/rubocop/d' Gemfile
  sed -i '/reek/d' Gemfile
  sed -i '/yard/d' Gemfile

  # remove gem signing
  sed -i '/cert_chain/d' ${_gemname}.gemspec
  sed -i '/signing_key/d' ${_gemname}.gemspec
}

build() {
  cd $_gemname-$_commit
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
  cd $_gemname-$_commit
  local _gemdir="$(gem env gemdir)"
  GEM_HOME="tmp_install/${_gemdir}" rake spec
}

package() {
  cd $_gemname-$_commit

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
