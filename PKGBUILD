# Maintainer: Mario Finelli <mario at finel dot li>
# Contributor: asm0dey <pavel.finkelshtein@gmail.com>

_gemname=tty-table
pkgname=ruby-$_gemname
pkgver=0.12.0
pkgrel=2
pkgdesc="A flexible and intuitive table generator"
arch=(any)
url=https://github.com/piotrmurach/tty-table
license=(MIT)
depends=(ruby ruby-pastel ruby-strings ruby-tty-screen)
checkdepends=(ruby-bundler ruby-rake ruby-rspec)
makedepends=(ruby-rdoc rubygems)
options=(!emptydirs)
source=(${url}/archive/v${pkgver}/$_gemname-$pkgver.tar.gz)
sha256sums=('d077a50b995a34f6e1bc1509f4aeaeb121cceb88e909d3544e1999d7b6bddbbe')

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
  GEM_HOME="tmp_install/${_gemdir}" rake spec
}

package() {
  cd $_gemname-$pkgver

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -v -Dm0644 LICENSE* "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -v -Dm0644 CHANGELOG.md -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -v -Dm0644 README.md -t "${pkgdir}/usr/share/doc/${pkgname}"
}

# vim: set ts=2 sw=2 et:
