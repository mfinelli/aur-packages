# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: Christopher Loen <christopherloen at gmail dot com>
# Contributor: Artem Vorotnikov <artem@vorotnikov.me>

_gemname=rspec_junit_formatter
pkgname=ruby-${_gemname}
pkgver=0.6.0
pkgrel=1
pkgdesc="RSpec results that your continuous integration service can read"
arch=(any)
url=https://github.com/sj26/rspec_junit_formatter
license=(MIT)
depends=(ruby ruby-rspec-core)
checkdepends=(ruby-bundler ruby-nokogiri ruby-rake ruby-coderay ruby-rspec)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(${url}/archive/v$pkgver/$_gemname-$pkgver.tar.gz)
sha256sums=('05ec70950f8dc3282c1fa49b8c0f828f10fd9280509e142dd1ffd720ccbd79ce')

prepare() {
  cd $_gemname-$pkgver
  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  sed -i 's|~>|>=|g' Gemfile

  sed -i '/appraisal/d' ${_gemname}.gemspec
  sed -i '/appraisal/d' Rakefile
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
  # GEM_HOME="tmp_install/${_gemdir}" rake spec
}

package() {
  cd $_gemname-$pkgver

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dvm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dvm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dvm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
