# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  Pellegrino Prevete <pellegrinoprevete at gmail.com>
# Maintainer:  Truocolo <truocolo at gmail.com>
# Contributor: mehalter <micah at mehalter.com>
# Contributor: xsmile <>
# Contributor: Pieter Frenssen <pieter@frenssen.be>
# Contributor: Will Drengwitz (ghtor) <ghthor@gmail.com>

_pkgname="myetherwallet"
pkgname="${_pkgname}-git"
_pkg="MyEtherWallet"
pkgver=6.8.1
pkgrel=1
_pkgdesc=(
  'Client-side interface for interacting'
  'with the Ethereum blockchain')
pkgdesc="${_pkgdesc[*]}"
arch=(
  any
)
depends=(
  xdg-utils
)
url="https://github.com/${_pkg}/${_pkg}"
license=(
  MIT
)
source=(
  "${_pkgname}-${pkgver}::git+${url}.git"
  "${_pkgname}.desktop"
  "${_pkgname}"
  "${_pkgname}.license")
sha256sums=(
  # '921af12de08b6f787bc815275034bacee59734851047b5a9d361cf8add2fc131'
  'SKIP'
  'b49e3bb0cded23c40820281a110d2baed5d1f7aa036635108323d0133e0e30e9'
  '814f489e4e6e0ce172513febf58d6821e7b77cd2e644152aec4888228fe41da5'
  '42039cf560b0b2b35463f33656d9639c8f9b98e9a4f57723dbab0a90a4ad8ad6')
validpgpkeys=(
  '2FCA4A1E3AF4278F7AD3B7637F059C0F7B9A12F0')

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    echo \
      "${_pkgver}" | \
          awk \
            -F '+' \
            '{print $1}')"
  _rev="$( \
    echo \
      "${_pkgver}" | \
      awk \
        -F '+' \
        '{print $2}')"
  _commit="$( \
    echo \
      "${_pkgver}" | \
      awk \
        -F '+' \
        '{print $3}')"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

pkgver() {
  local \
    _pkgver
  cd \
    "${_pkgname}-${pkgver}"
  _pkgver="$( \
    git \
      describe \
      --tags | \
      sed \
        's/-/+/g')"
  __pkgver="${pkgver}"
  _parse_ver \
    "${_pkgver}"
}

prepare() {
  tree \
    -d \
      2 \
    "${srcdir}" || \
    ls \
      "${srcdir}"
  mv \
    "${_pkgname}-${__pkgver}" \
    "${_pkgname}-${pkgver}" || \
  true
  # Remove JS source maps
  find \
    . \
    -name "*.map" \
    -delete
  # Remove development versions of JS files
  find \
    . \
    -name "*.dev.js" \
    -delete
}

package() {
  # Lib directory
  install \
    -d "${pkgdir}/opt/${_pkgname}"
  cp \
    -a \
    css \
    fonts \
    img \
    js \
    workbox-* \
    *.css \
    *.html \
    *.js \
    *.png \
    "${pkgdir}/opt/${_pkgname}"
  # Executable
  install \
    -Dm755 "${srcdir}/${_pkgname}" \
    -t "${pkgdir}/usr/bin"
  # License
  install \
    -Dm644 "${srcdir}/${_pkgname}.license" \
    -t "${pkgdir}/usr/share/licenses/${_pkgname}"
  # Menu entry and icon
  install \
    -Dm644 "${srcdir}/${_pkgname}.desktop" \
    -t "${pkgdir}/usr/share/applications"
  install \
    -Dm644 favicon.png \
    "${pkgdir}/usr/share/pixmaps/${_pkgname}.png"
}

# vim:set sw=2 sts=-1 et:
