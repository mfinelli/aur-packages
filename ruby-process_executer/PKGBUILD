# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=process_executer
pkgname=ruby-$_gemname
pkgver=4.0.0
pkgrel=1
pkgdesc="An API for executing commands in a subprocess"
arch=(any)
url=https://github.com/main-branch/process_executer
license=(MIT)
depends=(ruby ruby-track_open_instances)
checkdepends=(ruby-rake ruby-rspec ruby-simplecov)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(${_gemname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz
        disable-simplecov-lcov.patch)
sha256sums=('cdfce23a303625b5c0f06831a38de78868c7ef3cc56b919a1ec2f393551f9433'
            '34bfe9ca48e8a06ed0eb0bf4fc1a41045cd03f0ab07394a751a359ee47255eb5')

prepare() {
  cd "${_gemname}-${pkgver}"

  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  sed -i 's|git ls-files -z|find -print0|' ${_gemname}.gemspec

  patch -p1 -N -i "$srcdir/disable-simplecov-lcov.patch"
}

build() {
  cd "${_gemname}-${pkgver}"

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
  cd "${_gemname}-${pkgver}"

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
