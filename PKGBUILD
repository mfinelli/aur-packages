# Maintainer: Mario Finelli <mario at finel dot li>

_gemname=oj
pkgname=ruby-$_gemname
pkgver=3.14.2
pkgrel=1
pkgdesc="The fastest JSON parser and object serializer"
arch=(i686 x86_64)
url=http://www.ohler.com/oj/
license=(MIT)
depends=(ruby)
checkdepends=(ruby-bundler ruby-rake ruby-rake-compiler ruby-minitest ruby-test-unit)
makedepends=(rubygems ruby-rdoc)
source=(https://github.com/ohler55/oj/archive/v$pkgver/$_gemname-$pkgver.tar.gz)
options=(!emptydirs)
sha256sums=('59bea6b693daa84c5d3414717f8461ba36fb9bcafac8ef0a38f677c0525be269')

prepare() {
  cd $_gemname-$pkgver

  # oj insists on running some tests with bundler
  sed -i 's/bundle exec//' Rakefile
}

build() {
  cd $_gemname-$pkgver
  gem build ${_gemname}.gemspec
}

check() {
  cd $_gemname-$pkgver
  rake
}

package() {
  cd $_gemname-$pkgver
  local _gemdir="$(gem env gemdir)"

  gem install \
    --ignore-dependencies \
    --no-user-install \
    -i "$pkgdir/$_gemdir" \
    -n "$pkgdir/usr/bin" \
    $_gemname-$pkgver.gem

  rm -rf "$pkgdir/$_gemdir/cache"

  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
