# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=rchardet
pkgname=ruby-$_gemname
pkgver=1.10.0
pkgrel=1
pkgdesc="Character encoding auto-detection in Ruby"
arch=(any)
url=https://github.com/jmhodges/rchardet
license=(LGPL-2.1-or-later)
depends=(ruby)
checkdepends=(ruby-bundler ruby-rake ruby-maxitest ruby-minitest)
makedepends=(ruby-rdoc rubygems)
options=(!emptydirs)
source=(${url}/archive/v$pkgver/$_gemname-$pkgver.tar.gz)
sha256sums=('c33019c1e16d234abe063ba1d99901aab95c9b606edf4c682c57f16a1186e47d')

prepare() {
  cd "$_gemname-$pkgver"
  # "LGPL" is invalid and so gem build complains
  # the license file in the readme is LGPL 2.1
  sed -i 's/"LGPL"/"LGPL-2.1-or-later"/' $_gemname.gemspec

  sed -i '/bump/d' Gemfile
  sed -i '/bump/d' Rakefile

  rm Gemfile.lock
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
  GEM_HOME="tmp_install/${_gemdir}" rake
}

package() {
  cd "$_gemname-$pkgver"

  cp --archive --verbose tmp_install/* "${pkgdir}"
  install -Dm0644 Readme.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
