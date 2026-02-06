# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=hashie
pkgname=ruby-$_gemname
pkgver=5.1.0
pkgrel=1
pkgdesc="a collection of classes and mixins that make Ruby hashes more powerful"
arch=(any)
url=https://github.com/hashie/hashie
license=(MIT)
depends=(ruby)
checkdepends=(ruby-rake ruby-rspec ruby-rspec-pending_for)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(https://github.com/hashie/hashie/archive/v$pkgver/$_gemname-$pkgver.tar.gz)
sha256sums=('33f3e58043bd1aa82c5eb44ee9313c4170c8b5ebddcf0ae0b9e4114a77931056')

prepare() {
  cd $_gemname-$pkgver

  sed -i 's|~>|>=|g' Gemfile

  sed -i '/benchmark/d' Gemfile
  sed -i '/guard/d' Gemfile
  sed -i '/pry/d' Gemfile
  sed -i '/rubocop/d' Gemfile
  sed -i '/danger/d' Gemfile
  sed -i '/simplecov/d' Gemfile

  sed -i '/rubocop/Id' Rakefile
  sed -i '/bundler/Id' Rakefile
  sed -i '/pry/d' spec/spec_helper.rb

  # https://github.com/hashie/hashie/pull/533
  sed -i "/require 'hashie'/a require 'json'" spec/spec_helper.rb
  sed -i "/require 'json'/a require 'tempfile'" spec/spec_helper.rb
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
  GEM_HOME="tmp_install/${_gemdir}" rake spec
}

package() {
  cd $_gemname-$pkgver

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dvm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dvm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dvm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
