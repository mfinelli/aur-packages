# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=track_open_instances
pkgname=ruby-$_gemname
pkgver=0.1.15
pkgrel=1
pkgdesc="A mixin to track instances of Ruby classes that require explicit cleanup"
arch=(any)
url=https://github.com/main-branch/track_open_instances
license=(MIT)
depends=(ruby)
makedepends=(git ruby-rdoc rubygems)
checkdepends=(ruby-bundler ruby-rake ruby-rspec ruby-simplecov)
options=(!emptydirs)
source=($pkgname::git+$url.git#tag=v$pkgver)
sha256sums=('b7acf65b2b0f4dc89da515ff1a57c45ca58fc4b9bd728cbe910969cade61fbaf')

prepare() {
  cd $pkgname
  sed -i 's|~>|>=|g' ${_gemname}.gemspec

  sed -i '/rubocop/d' ${_gemname}.gemspec
  sed -i '/simplecov/d' ${_gemname}.gemspec
  sed -i '/create_github_release/d' ${_gemname}.gemspec
  sed -i '/bundler-audit/d' ${_gemname}.gemspec
  sed -i '/yard/d' ${_gemname}.gemspec
  sed -i '/redcarpet/d' ${_gemname}.gemspec

  sed -i '/SimpleCov::Formatter::/d' spec/spec_helper.rb
  sed -i '/simplecov-/d' spec/spec_helper.rb
  sed -i '/SimpleCov::RSpec/d' spec/spec_helper.rb
}

build() {
  cd $pkgname
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
  cd $pkgname
  local _gemdir="$(gem env gemdir)"
  GEM_HOME="tmp_install/${_gemdir}" rspec
}

package() {
  cd $pkgname

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -v -Dm0644 LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -v -Dm0644 CHANGELOG.md -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -v -Dm0644 README.md -t "${pkgdir}/usr/share/doc/${pkgname}"
}

# vim: set ts=2 sw=2 et:
