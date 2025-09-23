# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: asm0dey <pavel.finkelshtein@gmail.com>

_gemname=strings
pkgname=ruby-$_gemname
pkgver=0.2.1
pkgrel=2
pkgdesc="A set of useful functions for transforming strings"
arch=(any)
url=https://github.com/piotrmurach/strings
license=(MIT)
options=(!emptydirs)
depends=(ruby ruby-strings-ansi ruby-unicode_utils ruby-unicode-display_width)
checkdepends=(ruby-bundler ruby-rake ruby-rspec)
makedepends=(rubygems ruby-rdoc)
source=(${url}/archive/v${pkgver}.tar.gz)
sha256sums=('45c144ba36aa30586cc8bd64e0398671deb8f0b7befd59005571890b5d88560d')

prepare() {
  cd $_gemname-$pkgver

  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  sed -i 's|<|>=|g' ${_gemname}.gemspec # force newer unicode-display_width

  # we don't care about coverage or metrics
  echo 'source "https://rubygems.org"' > Gemfile
  echo 'gemspec' >> Gemfile
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
