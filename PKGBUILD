# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=ipsort
pkgver=1.0.0
pkgrel=1
pkgdesc="versitile ip address sorting tool"
arch=(x86_64)
url=https://github.com/mfinelli/ipsort
license=(GPL-3.0-or-later)
makedepends=(cargo scdoc)
depends=(glibc libgcc)
options=(!lto)
source=($pkgname-$pkgver.tar.gz::$url/archive/v$pkgver/$pkgname-$pkgver.tar.gz
  $pkgname.patch::$url/commit/ec61159533856d09992315427765d02641825788.patch)
sha256sums=('656c533f39dcd8ac1ffdfe001974637a37bc5976eb44e4400ecb025c05defb17'
            'abbc6b3eb91446ebc8c17b1597bd985ef0678f0ab450f24a55bf757012874460')

prepare() {
  cd $pkgname-$pkgver
  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$(rustc --print host-tuple)"
  patch -p1 -N -i "$srcdir/$pkgname.patch"
}

build() {
  cd $pkgname-$pkgver
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  make
}

check() {
  cd $pkgname-$pkgver
  export RUSTUP_TOOLCHAIN=stable
  cargo test --frozen
}

package() {
  cd $pkgname-$pkgver
  make install DESTDIR="$pkgdir" PREFIX=/usr
  install -Dm0644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm0644 CHANGELOG.md "$pkgdir/usr/share/doc/$pkgname/CHANGELOG.md"
}

# vim: set ts=2 sw=2 et:
