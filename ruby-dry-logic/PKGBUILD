# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: George Rawlinson <grawlinson@archlinux.org>

_gemname=dry-logic
pkgname=ruby-$_gemname
pkgver=1.5.0
pkgrel=1
pkgdesc="Predicate logic with rule composition"
arch=(any)
url=https://github.com/dry-rb/dry-logic
license=(MIT)
depends=(ruby ruby-concurrent ruby-dry-core ruby-zeitwerk)
checkdepends=(ruby-bundler ruby-rake ruby-rspec ruby-warning)
makedepends=(rubygems ruby-rdoc)
options=(!emptydirs)
source=(${_gemname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz
        ruby30fix.patch::https://patch-diff.githubusercontent.com/raw/dry-rb/dry-logic/pull/90.patch)
sha256sums=('9d772d3ff15982ecf349d34f3a9513bb6fb399da359de4df9dfb4603b42062ec'
            '38d4b934c28aa915e44a9075174f6259f24877283f1eb487394895c9faa699ad')

prepare() {
  cd "${_gemname}-${pkgver}"

  patch -p1 -N -i "${srcdir}/ruby30fix.patch"

  sed -i 's|~>|>=|g' ${_gemname}.gemspec
  sed -i '/cobertura/d' Gemfile.devtools
  sed -i '/rubocop/d' Gemfile.devtools # repo rubocop is too old
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
