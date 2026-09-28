# Maintainer: Ibrahim Rafi <rafiibrahim8 at hotmail dot com>

pkgname=bwphone-bin
_pkgname=bwphone
pkgver=0.1.0
pkgrel=1
pkgdesc="Unlock the Bitwarden browser extension with your phone's fingerprint (prebuilt binary)"
arch=('x86_64')
url="https://github.com/rafiibrahim8/bwphone"
license=('MIT')
depends=('gcc-libs' 'glibc' 'org.freedesktop.secrets' 'systemd')
optdepends=('bitwarden-cli: enrolling an account (bwphone enroll)')
provides=("${_pkgname}")
conflicts=("${_pkgname}")
options=(!strip !debug)
install="${pkgname}.install"
source=("${_pkgname}-${pkgver}-linux-x86_64.tar.gz::${url}/releases/download/v${pkgver}/${_pkgname}-v${pkgver}-linux-x86_64.tar.gz")
sha256sums=('056b1297ee5f10204ac5ddd5f4b74d2f43e46142857201a0eebc5d3f8733a674')

package() {
  cd "${_pkgname}-v${pkgver}-linux-x86_64"

  # The command on PATH; the browser-spawned proxy and the hello service in a private lib dir.
  install -Dm755 bwphone "${pkgdir}/usr/bin/bwphone"
  install -Dm755 bwphone-proxy bwphone-hello -t "${pkgdir}/usr/lib/${_pkgname}/"

  # systemd user units, with the per-user paths rewritten for /usr (as the upstream installer does).
  install -dm755 "${pkgdir}/usr/lib/systemd/user"
  for unit in bwphone bwphone-hello; do
    sed -e "s|%h/.local/bin/|/usr/bin/|" -e "s|%h/.local/lib/bwphone/|/usr/lib/${_pkgname}/|" \
      "systemd/${unit}.service" > "${pkgdir}/usr/lib/systemd/user/${unit}.service"
    chmod 644 "${pkgdir}/usr/lib/systemd/user/${unit}.service"
  done

  install -Dm644 man/*.1 -t "${pkgdir}/usr/share/man/man1/"
  install -Dm644 completions/bwphone.bash "${pkgdir}/usr/share/bash-completion/completions/bwphone"
  install -Dm644 completions/_bwphone "${pkgdir}/usr/share/zsh/site-functions/_bwphone"
  install -Dm644 completions/bwphone.fish "${pkgdir}/usr/share/fish/vendor_completions.d/bwphone.fish"

  install -Dm644 README.md "${pkgdir}/usr/share/doc/${_pkgname}/README.md"
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
