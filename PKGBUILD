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
_clang="true"
_avx="false"
if [[ "${_clang}" == "true" ]]; then
  _cc="clang"
  _cxx="clang++"
  _ld="lld"
fi
_wayland="true"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _wayland="false"
fi
_git="true"
_pkg=pcsx2
_Pkg="PCSX2"
pkgname="${_pkg}-1.7"
pkgver="1.7.4592"
_commit="b6923f49b159303bd3a2281021d22cdb6b8ea308"
_ffmpeg_ver="6.1"
# pkgver="2.2"
#_commit="2d5faa627ff54f3fb2a69a43286181bee071a1c3"
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
  "ffmpeg<7"
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
  # new now-removed dependency
  # 'shaderc-non-semantic-debug'
  'soundtouch'
  'xz'
  'zlib'
)
if [[ "${_wayland}" == "true" ]]; then
  depends+=(
    'wayland'
  )
  makedepends+=(
    'qt6-wayland'
  )
fi
# this specific version dependencies
depends+=(
  'libasound.so'
  'libfmt.so'
  'libsamplerate.so'
  'libzip.so'
)
makedepends=(
  "${_cc}"
  'cmake'
  # new dependency not currently needed
  # 'extra-cmake-modules'
  # new dependency
  # 'libpipewire'
  'libpulse'
  "${_ld}"
  'llvm'
  'ninja'
  'p7zip'
  'qt6-tools'
)
optdepends=(
  'qt6-wayland: Wayland support'
  'libpipewire: Pipewire support'
  'libpulse: PulseAudio support'
)
provides+=(
  "${_pkg}=${pkgver}"
)
_tag_name="commit"
_tag="${_commit}"
options=(
  !lto
)
_http="https://github.com"
_ns="${_Pkg}"
_url="${_http}/${_ns}/${_pkg}"
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
  source=(
    "git+${_url}.git#${_tag_name}=${_tag}"
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
  # This version extra sources
  source+=(
    "git+${_http}/rtissera/libchdr.git"
    "git+${_http}/KhronosGroup/glslang.git"
    "xz-pcsx2::git+${_http}/${_ns}/xz.git"
    "git+${_http}/nih-at/libzip.git"
    "git+${_http}/facebook/zstd.git"
    "git+${_http}/RetroAchievements/rcheevos.git"
  )
  b2sums=(
    'SKIP'
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
  # This version extra sums
  b2sums+=(
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
    # Extra submodules for this version
    "xz-pcsx2::3rdparty/xz/xz"
    "glslang::3rdparty/glslang/glslang"
    "libchdr::3rdparty/libchdr/libchdr"
    "libzip::3rdparty/libzip/libzip"
    "zstd::3rdparty/zstd/zstd"
    "rcheevos::3rdparty/rcheevos/rcheevos"
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
    -C "${_pkg}" \
    describe \
      --tags | \
    sed \
      's/^v//'
}

_usr_get() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$(command \
           -v \
	   "env")")"
  dirname \
    "${_bin}"
}

build() {
  local \
    _cmake_opts=() \
    _cxxflags=() \
    _wayland_api \
    _avx_disabled
  _cxxflags=(
    $CXXFLAGS
  )
  if [[ "${_clang}" == "true" ]]; then
    _cxxflags+=(
      -Wp,-D_FORTIFY_SOURCE=0
      -Wno-deprecated-declarations
    )
  fi
  if [[ "${_wayland}" == "true" ]]; then
    _wayland_api="ON"
  elif [[ "${_wayland}" == "false" ]]; then
    _wayland_api="OFF"
  fi
  if [[ "${_avx}" == "true" ]]; then
    _avx_disabled="FALSE"
  elif [[ "${_avx}" == "false" ]]; then
    _avx_disabled="TRUE"
  fi
  _cmake_opts+=(
    -S
      "${_pkg}"
    -B
      "build"
    -G
      "Ninja"
    -DCMAKE_C_COMPILER="${_cc}"
    -DCMAKE_CXX_COMPILER="${_cxx}"
    -DCMAKE_EXE_LINKER_FLAGS_INIT="-fuse-ld=${_ld}"
    -DCMAKE_MODULE_LINKER_FLAGS_INIT="-fuse-ld=${_ld}"
    -DCMAKE_SHARED_LINKER_FLAGS_INIT="-fuse-ld=${_ld}"
    -DCMAKE_INTERPROCEDURAL_OPTIMIZATION="ON"
    -DCMAKE_BUILD_TYPE="Release"
    -DX11_API="ON"
    -DWAYLAND_API="${_wayland_api}"
    -DENABLE_SETCAP="OFF"
    -DDISABLE_ADVANCE_SIMD="${_avx_disabled}"
    -DCMAKE_INSTALL_PREFIX="/usr"
    -DCMAKE_CXX_STANDARD_INCLUDE_DIRECTORIES="$(_usr_get)/include/ffmpeg${_ffmpeg_ver}"
    -DCMAKE_CXX_FLAGS="-L$(_usr_get)/lib/ffmpeg${_ffmpeg_ver}"
    -DFFMPEG_INCLUDE_DIRS="$(_usr_get)/include/ffmpeg${_ffmpeg_ver}"
    -DFFMPEG_LIBRARIES="$(_usr_get)/lib/ffmpeg${_ffmpeg_ver}"
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
  CXXFLAGS="${_cxxflags[*]}" \
  cmake \
    "${_cmake_opts[@]}"
  CXXFLAGS="${_cxxflags[*]}" \
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
/opt/${pkgname}/${_pkg}-qt "\$@"
eof
  cd \
    "${_pkg}"
  install \
    -Dm644 \
    ".github/workflows/scripts/linux/${_pkg}-qt.desktop" \
    -t \
    "${pkgdir}/usr/share/applications/"
  sed \
    "s/Icon=${_Pkg}/Icon=${pkgname}/" \
    -i \
    "${pkgdir}/usr/share/applications/${_pkg}-qt.desktop"
  install \
    -Dm644 \
    "bin/resources/icons/AppIconLarge.png" \
    "${pkgdir}/usr/share/icons/hicolor/64x64/apps/${pkgname}.png"
}
