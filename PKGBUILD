# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: asm0dey <pavel.finkelshtein@gmail.com>

_gemname=tty-reader
pkgname=ruby-$_gemname
pkgver=0.9.0
pkgrel=2
pkgdesc="A set of methods for processing keyboard input"
arch=(any)
url=https://github.com/piotrmurach/tty-reader
license=(MIT)
options=(!emptydirs)
depends=(ruby ruby-wisper ruby-tty-screen ruby-tty-cursor)
checkdepends=(ruby-bundler ruby-rake ruby-rspec)
makedepends=(rubygems ruby-rdoc)
source=(${url}/archive/v${pkgver}.tar.gz
  https://patch-diff.githubusercontent.com/raw/piotrmurach/tty-reader/pull/32.patch)
sha256sums=('970cab15cfbe28388ec6f86b16b02c09c062be90b84de8ece4a5c1e1db69b555'
            'ac1be0c8bec8d9f094d3b82ba4cf51ba5ccf51ddbd941548407efa8afe58b61c')

prepare() {
  cd $_gemname-$pkgver

  patch -p1 -N -i "${srcdir}/32.patch"

  # we don't care about performance or metrics
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
  GEM_HOME="tmp_install/${_gemdir}" rake
}

package() {
  cd $_gemname-$pkgver
  local _gemdir="$(gem env gemdir)"

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
