# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=buftok
pkgname=ruby-$_gemname
pkgver=1.0.1
pkgrel=1
pkgdesc="extract token delimited entities from a sequence of arbitrary inputs"
arch=(any)
url=https://github.com/sferik/buftok
license=(MIT)
depends=(ruby)
checkdepends=(ruby-bundler ruby-minitest ruby-rake ruby-simplecov)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(https://github.com/sferik/buftok/archive/v$pkgver/$_gemname-$pkgver.tar.gz
  easy-rake.patch)
sha256sums=('56c2b6552acad11232154003056d31c98fef09d480ae224111112421df8d8516'
            '815c09fe021098a420f12c97c335455bc7ee89b0a1bd7105b82144d664270a1d')

prepare() {
  cd $_gemname-$pkgver
  sed -i 's|~>|>=|g' Gemfile

  # we don't care about any of these new dev dependencies we just want to run
  # the tests
  sed -i '/mutant/Id' Gemfile
  sed -i '/rubocop/Id' Gemfile
  sed -i '/standard/Id' Gemfile
  sed -i '/standard/Id' Gemfile
  sed -i '/steep/Id' Gemfile
  sed -i '/yard/Id' Gemfile

  patch -p1 -N -i "$srcdir/easy-rake.patch"
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
  GEM_HOME="tmp_install/${_gemdir}" rake test
}

package() {
  cd $_gemname-$pkgver
  local _gemdir="$(gem env gemdir)"

  cp --archive --verbose tmp_install/* "${pkgdir}"
  install -Dm0644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
