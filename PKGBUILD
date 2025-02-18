# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
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
# Maintainer: éclairevoyant
# Contributor: rafaelff <rafaelff at gnome dot org>
# Contributor: WeirdBeard <obarrtimothy at gmail dot com>
# Contributor: Themaister <maister at archlinux dot us>
# Contributor: Maxime Gauduin <alucryd at archlinux dot org>
# Contributor: josephgbr <rafael dot f dot f1 at gmail dot com>
# Contributor: vEX <vex at niechift dot com>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_git="true"
_pkg=pcsx2
_Pkg="PCSX2"
pkgname="${_pkg}"
pkgver=2.2
_commit="7bc5427908a0e2985bd354446e4bfeeedd4cd45b"
pkgrel=1
pkgdesc='A Sony PlayStation 2 emulator'
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'mips'
  'i686'
  'pentium4'
  'armv7l'
  'powerpc'
)
url="https://www.${_pkg}.net"
license=(
  'GPL2'
  'GPL3'
  'LGPL2.1'
  'LGPL3'
)
depends=(
  'ffmpeg'
  "${_libc}"
  'libaio.so'
  'libgl'
  'libharfbuzz.so'
  'libpcap.so'
  'libpng'
  'libudev.so'
  'libx11'
  'libxcb'
  'libxrandr'
  'libzstd.so'
  'qt6-base'
  'qt6-svg'
  'sdl2'
  'shaderc-non-semantic-debug'
  'soundtouch'
  'wayland'
  'xz'
  'zlib'
)
makedepends=(
  'clang'
  'ccmake'
  'cextra-cmake-modules'
  'cgit'
  'clibpipewire'
  'clibpulse'
  'clld'
  'cllvm'
  'cninja'
  'cp7zip'
  'cqt6-tools'
  'cqt6-wayland'
)
optdepends=(
  'qt6-wayland: Wayland support'
  'libpipewire: Pipewire support'
  'libpulse: PulseAudio support'
)
_tag="${_commit}"
options=(
  !lto
)
_http="https://github.com"
_ns="${_Pkg}"
_url="${_http}/${_ns}/${_pkg}"
if [[ "${_git}" == "true" ]]; then
  source=(
    "git+${_url}.git#tag=${_tag}"
    "git+${_http}/${_ns}/${_pkg}_patches.git"
    "git+${_http}/google/googletest.git"
    "git+${_http}/fmtlib/fmt.git"
    "git+${_http}/biojppm/rapidyaml.git"
    "git+${_http}/biojppm/cmake.git"
    "git+${_http}/biojppm/c4core.git"
    "git+${_http}/biojppm/debugbreak.git"
    "git+${_http}/fastfloat/fast_float.git"
    "vulkan-headers::git+${_http}/KhronosGroup/Vulkan-Headers.git"
  )
  b2sums=(
    '4e7df739987b26a0af09bbc807355eca2f7c3d7df671abbcdba50325f449dfa2c6db2bb63c093af93e934a3ff00e55407ad77918823afb60992b39ce8dfbc3c6'
    'SKIP'
    'SKIP'
    'SKIP'
    'SKIP'
    'SKIP'
    'SKIP'
    'SKIP'
    'SKIP'
    'SKIP'
  )
fi
install="${_pkg}.install"

_git_submodule_update() {
  local \
    _path="${1}" \
    _url="${2}"
  git \
    submodule \
      init \
        "${_path}"
  git \
    submodule \
      set-url \
        "${_path}" \
        "${_url}"
  git \
    -c \
      protocol.file.allow="always" \
    submodule \
      update \
        "${_path}"
}

prepare() {
  local \
    _submodule
  cd \
    "${_pkg}"
  _pcsx2_submodules=(
    "googletest::3rdparty/gtest"
    "fmt::3rdparty/fmt/fmt"
    "rapidyaml::3rdparty/rapidyaml/rapidyaml"
    "vulkan-headers::3rdparty/vulkan-headers"
  )
  # must be done this way due to recursive submodules
  for _submodule \
    in ${_pcsx2_submodules[@]}; do
    _git_submodule_update \
      "${_submodule#*::}" \
      "${srcdir}/${_submodule%::*}"
  done
  cd \
    "3rdparty/rapidyaml/rapidyaml"
  for _submodule in "ext/c4core"; do
    _git_submodule_update \
      "${_submodule}" \
      "${srcdir}/${_submodule##*/}"
  done
  cd \
    "ext/c4core"
  for _submodule \
    in "cmake" "src/c4/ext/"{"debugbreak","fast_float"}; do
    _git_submodule_update \
      "${_submodule}" \
      "${srcdir}/${_submodule##*/}"
  done
}

pkgver() {
  git \
    -C "${pkgname}" \
    describe \
      --tags | \
    sed \
      's/^v//'
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts+=(
    -S
      "${pkgname}"
    -B
      "build"
    -G
      "Ninja"
    -DCMAKE_C_COMPILER="clang"
    -DCMAKE_CXX_COMPILER="clang++"
    -DCMAKE_EXE_LINKER_FLAGS_INIT="-fuse-ld=lld"
    -DCMAKE_MODULE_LINKER_FLAGS_INIT="-fuse-ld=lld"
    -DCMAKE_SHARED_LINKER_FLAGS_INIT="-fuse-ld=lld"
    -DCMAKE_INTERPROCEDURAL_OPTIMIZATION="ON"
    -DCMAKE_BUILD_TYPE="Release"
    -DX11_API="ON"
    -DWAYLAND_API="ON"
    -DENABLE_SETCAP="OFF"
    -DDISABLE_ADVANCE_SIMD="TRUE"
    -DCMAKE_INSTALL_PREFIX="/usr"
  )
  # 7zip the patches
  pushd \
    "pcsx2_patches"
  7z \
    a \
    -r \
    "${srcdir}/patches.zip" \
    "patches/."
  popd
  # see .github/workflows/scripts/linux/generate-cmake-qt.sh
  cmake \
    "${_cmake_opts[@]}"
  ninja \
    -C "build" \
    -v
}

package() {
  install \
    -Dm644 \
    "patches.zip" \
    -t \
    "${pkgdir}/opt/${pkgname}/resources/"
  cp \
    -r \
    "build/bin/"* \
    "${pkgdir}/opt/${pkgname}/"
  install \
    -Dm755 \
    "/dev/stdin" \
    "${pkgdir}/usr/bin/${_pkg}-qt" <<eof
#!/usr/bin/env sh
/opt/${_pkg}/${_pkg}-qt "\$@"
eof
  cd \
    "${pkgname}"
  install \
    -Dm644 \
    ".github/workflows/scripts/linux/${_pkg}-qt.desktop" \
    -t \
    "${pkgdir}/usr/share/applications/"
  install \
    -Dm644 \
    "bin/resources/icons/AppIconLarge.png" \
    "${pkgdir}/usr/share/icons/hicolor/64x64/apps/${pkgname}.png"
}
