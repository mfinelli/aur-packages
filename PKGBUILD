# Maintainer: Mario Finelli <mario at finel dot li>

pkgname=foobar2000
pkgver=2.24.1
pkgrel=1
pkgdesc="An advanced freeware audio player (uses Wine)"
arch=(i686 x86_64)
url=https://www.foobar2000.org
license=(custom)
depends=(wine desktop-file-utils)
makedepends=(7zip wget)
source_i686=("foobar2000_v$pkgver.exe::${url}/download")
source_x86_64=("foobar2000-x64_v$pkgver.exe::${url}/download")
source=("LICENSE"
        "${pkgname}.sh"
        "${pkgname}.png"
        "${pkgname}.desktop")
install=$pkgname.install
sha256sums=('134cf36d7631628bfa4a3830704aa6a9efb6e265660b31d5b487df97aaa32721'
            'e9d1f7a8dc182f780d0fb15818c2837ebe0b70abcbef1c849a4185da7c09be7c'
            '2031e952d1d1d6cb4c2ff2b879421149f3f0780ca5d3ac03bc9c23fcbbd053d6'
            '2b6134997e55ce5ef5014dc266cda71b763949ad683d321246a5143333dd1fa0')
sha256sums_i686=('139b9932575e8a366bdbfcfa50d0f58d1cd4d6c3931718095ca80ad21fb5affd')
sha256sums_x86_64=('6fdef47dd10bea286bc3c95f2e6d87dc527382d12831acdddb0a450f6b35c0c6')
DLAGENTS=('https::/usr/bin/wget -nH --cut-dirs=3 -r -l 2 -A exe -R '*beta*.exe' -R '*arm*' %u')

package() {
  if [[ "$CARCH" == "x86_64" ]]; then
    _source_exe="${pkgname}-x64_v${pkgver}.exe"
  else
    _source_exe="${pkgname}_v${pkgver}.exe"
  fi

  # unpack NSIS installer .exe into destination
  7z x "${_source_exe}" -x'!$PLUGINSDIR' -x'!$R0' -x'!icons' \
    -x'!foobar2000 Shell Associations Updater.exe' \
    -o"${pkgdir}/usr/share/${pkgname}"

  # correct filesystem permissions
  find "${pkgdir}" -type d -execdir chmod 755 {} +

  # run foobar2000 in portable mode
  touch "${pkgdir}/usr/share/${pkgname}/portable_mode_enabled"

  install -Dm0644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -Dm0755 ${pkgname}.sh "${pkgdir}/usr/bin/${pkgname}"
  install -Dm0644 ${pkgname}.png "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
  install -Dm0644 ${pkgname}.desktop \
    "${pkgdir}/usr/share/applications/${pkgname}.desktop"
}

# vim: set ts=2 sw=2 et:
