# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=rspec-pending_for
pkgname=ruby-${_gemname}
pkgver=0.1.19
pkgrel=1
pkgdesc="Mark specs pending or skipped for specific Ruby engine / version"
arch=(any)
url=https://github.com/pboling/rspec-pending_for
license=(MIT)
depends=(ruby ruby-rspec-core ruby-ruby_engine ruby-ruby_version)
checkdepends=(ruby-bundler ruby-byebug ruby-minitest ruby-pry ruby-rake
              ruby-redcarpet ruby-rspec ruby-rspec-block_is_expected
              ruby-rspec_junit_formatter ruby-rubocop ruby-simplecov
              ruby-test-unit ruby-yard)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(${url}/archive/v$pkgver/$_gemname-$pkgver.tar.gz)
sha256sums=('adee5bfd1a07abcdbf1ebc77d884eebc75ce205939f8157a84075ae1a03edba7')

prepare() {
  cd $_gemname-$pkgver
  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  sed -i 's|~>|>=|g' Gemfile

  sed -i '/github-markup/d' Gemfile

  # remove gem signing stuff
  sed -i '/default_user_cert = /,+10d' ${_gemname}.gemspec
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
  GEM_HOME="tmp_install/${_gemdir}" rspec
}

package() {
  cd $_gemname-$pkgver

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
