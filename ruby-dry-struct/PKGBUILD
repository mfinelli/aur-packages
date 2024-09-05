# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: George Rawlinson <grawlinson@archlinux.org>

_gemname=dry-struct
pkgname=ruby-$_gemname
pkgver=1.6.0
pkgrel=1
pkgdesc="Typed struct and value objects"
arch=(any)
url=https://github.com/dry-rb/dry-struct
license=(MIT)
depends=(ruby ruby-dry-core ruby-dry-types ruby-ice_nine ruby-zeitwerk)
checkdepends=(ruby-bundler ruby-dry-monads ruby-rake ruby-rspec ruby-yard
              ruby-warning)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(${_gemname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz)
sha256sums=('082c7349bbf94f942238f6eaf92ceed11d6b5b71c6be60728d9604250f06122b')

prepare() {
  cd "${_gemname}-${pkgver}"

  sed -i 's|~>|>=|g' ${_gemname}.gemspec
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
  GEM_HOME="tmp_install/${_gemdir}" rake
}

package() {
  cd "${_gemname}-${pkgver}"

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}

# vim: set ts=2 sw=2 et:
