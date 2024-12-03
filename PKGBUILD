# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=rantly
pkgname=ruby-$_gemname
pkgver=3.0.0
pkgrel=1
pkgdesc="Ruby Imperative Random Data Generator and Quickcheck"
arch=(any)
url=https://github.com/rantly-rb/rantly
license=(MIT)
options=(!emptydirs)
depends=(ruby)
checkdepends=(ruby-minitest ruby-rake ruby-simplecov ruby-rubocop ruby-rubocop-performance)
makedepends=(rubygems ruby-rdoc)
source=(${url}/archive/${pkgver}.tar.gz
        https://patch-diff.githubusercontent.com/raw/rantly-rb/rantly/pull/86.patch)
sha256sums=('ecd37ad5541a914e52987165754d087dee0a677c9aa8d2c62f68849e8f34d143'
            '50982670fda8d2e3f2958670d52786a6542d43a6cbf38b84a037fb2f7677e02c')

prepare() {
  cd $_gemname-$pkgver
  sed -i '/coveralls/d' Gemfile
  sed -i 's|~>|>=|g' Gemfile

  patch -p1 -N -i "${srcdir}/86.patch"
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

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
