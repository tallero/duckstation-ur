# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2022, 2023, 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributor: katt <magunasu.b97@gmail.com>

_evmfs_available="$( \
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi

if [[ ! -v "_git" ]]; then
  _git="true"
fi
_pkg=duckstation
_pkgname="${_pkg}"
pkgname="${_pkg}"
pkgver=2023.06.08
_pkgver="latest"
pkgdesc=(
  "A Sony PlayStation (PSX) emulator,"
  "focusing on playability, speed,"
  "and long-term maintainability"
)
pkgdesc="${_pkgdesc[*]}"
_commit="2d78b3f26a18600cbeb1f7add97f345d7345deeb"
pkgrel=1
arch=(
  "x86_64"
  "aarch64"
  "i686"
  "pentium4"
)
_http="https://github.com"
# _ns="stenzek"
_ns="themartiancompany"
url="${_http}/${_ns}/${_pkg}"
license=(
  "GPL3"
)
depends=(
  "sdl2"
  "qt6-base"
)
makedepends=(
  "cmake"
  "extra-cmake-modules"
  "qt6-tools"
  "libdrm"
  "libpulse"
  "alsa-lib"
  "sndio"
  "gtk3"
  "ninja"
)
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
_psx_bios_optdepends=(
  "psx-bios:"
    "Sony PlayStation bioses."
)
optdepends=(
  "${_psx_bios_optdepends[*]}"
)
_tarname="${_pkg}-${_pkgver}"
if [[ "${_git}" == "true" ]]; then
  _src="${_tarname}::git+${url}#commit=${_commit}"
  _sum="SKIP"
elif [[ "${_git}" == "false" ]]; then
  _uri="${url}/archive/refs/tags/${_pkgver}.tar.gz"
  _src="${_tarname}.tar.xz::${_uri}"
  _sum='adc6af10f1a14059ebb00637dac7283760f6ef647ebaec224a0e6e88ac901f0a'
fi
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)

build() {
  local \
    _cmake_opts=()
  _cmake_opts+=(
    -B
      "build"
    -S \
      "${_tarname}"
    -DBUILD_NOGUI_FRONTEND="ON"
    -DUSE_WAYLAND="ON"
    -G
      Ninja
    -Wno-dev
  )
  cmake \
    "${_cmake_opts[@]}"
  ninja \
    -C \
      build
}

package() {
  # Main files
  install \
    -dm755 \
    "${pkgdir}/opt"
  cp \
    -rv \
    "build/bin" \
    "${pkgdir}/opt/${_pkg}"
  # Symlink to /usr/bin
  install \
    -dm755 \
    "${pkgdir}/usr/bin"
  ln \
    -svt \
    "${pkgdir}/usr/bin" \
    "/opt/${_pkg}/${_pkg}"-{"qt","nogui"}
  # Desktop file
  cat > \
    "${_tarname}/data/resources/${_pkg}.desktop" << EOF
[Desktop Entry]
Type=Application
Name=DuckStation
GenericName=PlayStation 1 Emulator
Comment=Fast PlayStation 1 emulator
Icon=duckstation
TryExec=duckstation-qt
Exec=duckstation-qt %f
Categories=Game;Emulator;Qt;
EOF
  install \
    -Dm644 \
    "${_tarname}/data/resources/${_pkg}.desktop" \
    "${pkgdir}/usr/share/applications/${_pkg}-qt.desktop"
  install \
    -Dm644 \
    "${_tarname}/data/resources/images/duck.png" \
    "${pkgdir}/usr/share/pixmaps/${_pkg}.png"
}
