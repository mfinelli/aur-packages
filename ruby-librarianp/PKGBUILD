# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=librarianp
pkgname=ruby-$_gemname
pkgver=1.2.0
pkgrel=1
pkgdesc="A Framework for Bundlers, used by librarian-puppet"
arch=(any)
url=https://github.com/voxpupuli/librarian
license=(MIT)
depends=(ruby ruby-thor)
checkdepends=(ruby-bundler ruby-rake ruby-rspec ruby-fakefs)
makedepends=(git rubygems ruby-rdoc)
options=(!emptydirs)
source=(git+https://github.com/voxpupuli/librarian.git#tag=v$pkgver
        fix-test.patch)
sha256sums=('59ac108407f7837d656d1951c2ed809b1b06f37c901a398796274ae5b01a4dbb'
            'd7801945f6258c5cbc6336df2127f9791f0947c2b1ddc30378e2c5cff6d5122d')

prepare() {
  cd librarian
  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  patch -p1 -N -i "$srcdir/fix-test.patch"
}

build() {
  cd librarian
  local _gemdir="$(gem env gemdir)"

  gem build ${_gemname}.gemspec

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
  cd librarian
  local _gemdir="$(gem env gemdir)"
  GEM_HOME="tmp_install/${_gemdir}" rake spec
}

package() {
  cd librarian

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim: set ts=2 sw=2 et:
