# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=git
pkgname=ruby-$_gemname
pkgver=4.0.6
pkgrel=1
pkgdesc="Ruby library to manipulate git repositories by wrapping system calls to the git binary"
arch=(any)
url=https://github.com/ruby-git/ruby-git
license=(MIT)
depends=(ruby git ruby-activesupport ruby-addressable ruby-process_executer
         ruby-rchardet)
checkdepends=(openssh ruby-bundler ruby-minitar ruby-mocha ruby-rake
              ruby-redcarpet ruby-rubocop ruby-test-unit ruby-yard)
makedepends=(rubygems ruby-rdoc)
source=(git+https://github.com/ruby-git/ruby-git.git#tag=v$pkgver
        disable-yardstick.patch)
sha256sums=('affabc597090827950efaca62430ee0a58c2542264fa9dccfbd3b63eb4bbc8f8'
            'a87ea4933af7bff13576d795c3ca3474e78347fd05d50a72ec7de65fdddc247e')

prepare() {
  cd ${pkgname}
  sed -i 's|~>|>=|g' ${_gemname}.gemspec

  patch -p1 -N -i "$srcdir/disable-yardstick.patch"
  sed -i -e '/yardstick/d' ${_gemname}.gemspec
  sed -i -e '/main_branch_shared_rubocop_config/d' ${_gemname}.gemspec

  sed -i -e '/create_github_release/d' ${_gemname}.gemspec
}

build() {
  cd ${pkgname}
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
  cd ${pkgname}
  local _gemdir="$(gem env gemdir)"
  # disable tests for now as a most of the checkdeps in the official repos are
  # too old
  # GEM_HOME="tmp_install/${_gemdir}" rake test
}

package() {
  cd ${pkgname}

  cp --archive --verbose tmp_install/* "${pkgdir}"

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
