# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: George Rawlinson <grawlinson@archlinux.org>

_gemname=dry-monads
pkgname=ruby-$_gemname
pkgver=1.6.0
pkgrel=1
pkgdesc="Useful, common monads in idiomatic Ruby"
arch=(any)
url=https://github.com/dry-rb/dry-monads
license=(MIT)
depends=(ruby ruby-concurrent ruby-dry-core ruby-zeitwerk)
checkdepends=(ruby-bundler ruby-dry-types ruby-rake ruby-rspec ruby-warning)
makedepends=(rubygems ruby-rdoc)
source=(${_gemname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz)
sha256sums=('39905d51132247b86449779b240bbee1188519025f98f129a0c1830f46fe8176')

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
