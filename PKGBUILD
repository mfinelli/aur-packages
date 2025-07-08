# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=nodejs-lhci-cli
pkgver=0.15.1
pkgrel=1
pkgdesc="The Lighthouse CI CLI"
arch=(any)
url=https://github.com/GoogleChrome/lighthouse-ci
license=(Apache-2.0)
depends=(nodejs)
makedepends=(npm)
source=(https://registry.npmjs.org/@lhci/cli/-/cli-$pkgver.tgz)
noextract=(cli-$pkgver.tgz)
sha256sums=('7d31d0916ca430d2ac50d4da5581fd3129b7b0485fb67c14bcc34c159cd8fb52')

package() {
  npm install --global \
    --cache "$srcdir/.npm-cache" \
    --prefix "$pkgdir/usr" \
    "$srcdir/cli-$pkgver.tgz"

  find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

  # npm gives ownership of ALL FILES to build user
  # https://bugs.archlinux.org/task/63396
  chown -R root:root "${pkgdir}"
}

# vim: set ts=2 sw=2 et:
