# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
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
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

_os="$( \
  uname \
    -o)"
_offline="false"
_termux="false"
_bash_completion="true"
_debug="false"
_docs='false'
_devel="true"
_checks='true'
_libmount='enabled'
_sysprof="enabled"
if [[ "${_debug}" == "false" ]]; then
  _dtrace="disabled"
  _glib_debug="disabled"
  _systemtap="disabled"
elif [[ "${_debug}" == "true" ]]; then
  _dtrace="enabled"
  _glib_debug="enabled"
  _systemtap="enabled"
fi
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _checks='false'
  _docs='false'
  _libc="ndk-sysroot"
  _libmount='disabled'
  _sysprof='disabled'
  _termux="true"
fi
_py="python"
_pkg="glib"
_proj="gnome"
_ns="GNOME"
pkgbase="${_pkg}2"
pkgname=(
  "${pkgbase}"
)
if [[ "${_devel}" == "true" ]]; then
  pkgname+=(
    "${pkgbase}-devel"
  )
fi
if [[ "${_docs}" == "true" ]]; then
  pkgname+=(
    "${pkgbase}-docs"
  )
fi
pkgver=2.82.4
pkgrel=1
pkgdesc="Low level core library"
_http="http://gitlab.${_proj}.org"
url="${_http}/${_ns}/${_pkg}"
_local="file://${HOME}/${_pkg}"
_local_gvdb="file://${HOME}/gvdb"
license=(
  'LGPL-2.1-or-later'
)
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'armv7l'
  'mips'
  'powerpc'
  'i686'
  'pentium4'
)
depends=(
  'bash'
  "${_libc}"
  'libffi'
  'pcre2'
  'zlib'
)
if [[ "${_termux}" == "true" ]]; then
  depends+=(
    "libandroid-support"
    "libiconv"
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  depends+=(
    'util-linux-libs'
  )
fi
if [[ "${_sysprof}" == "enabled" ]]; then
  depends+=(
    'libsysprof-capture'
)
fi
makedepends=(
  'dbus'
  'dconf'
  'gettext'
  'git'
  'gi-docgen'
  'gtk-doc'
  'gobject-introspection'
  'libelf'
  'meson'
  "${_py}"
  "${_py}-docutils"
  "${_py}-packaging"
  'shared-mime-info'
  'util-linux'
)
if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    'g-ir-scanner'
  )
fi
if [[ "${_bash_completion}" == "true" ]]; then
  makedepends+=(
    "bash-completion"
  )
fi
checkdepends=(
  'desktop-file-utils'
  "${_pkg}2"
)
options=(
  'staticlibs'
)
if [[ "${_debug}" == "true" ]]; then
  options+=(
    'debug'
  )
fi
_url="${url}"
_gvdb_url="${_http}/${_ns}/gvdb"
if [[ "${_offline}" == "true" ]]; then
  _url="${_local}"
  _gvdb_url="${_local_gvdb}"
fi
_commit="ca20e4ac71864f08e980dc044ac96c06d5482b37"  # tags/2.82.4^0
source=(
  "git+${_url}.git#commit=$_commit"
  "git+${_gvdb_url}"
  "0001-${_pkg}-compile-schemas-Remove-noisy-deprecation-warnin.patch"
  "gio-querymodules.hook"
  "${_pkg}-compile-schemas.hook"
)
b2sums=(
  'SKIP'
  'SKIP'
  '94c73ca7070c239494873dd52d6ee09382bbb5b1201f7afd737cfa140b1a2fb0744b2c2831baf3943d1d072550c35888d21ce6f19f89481ff9d1a60d9a0b30e0'
  '14c9211c0557f6d8d9a914f1b18b7e0e23f79f4abde117cb03ab119b95bf9fa9d7a712aa0a29beb266468aeb352caa3a9e4540503cfc9fe0bbaf764371832a96'
  'd30d349b4cb4407839d9074ce08f5259b8a5f3ca46769aabc621f17d15effdb89c4bf19bd23603f6df3d59f8d1adaded0f4bacd0333afcab782f2d048c882858'
)

pkgver() {
  cd \
    "${_pkg}"
  git \
    describe \
      --tags | \
  sed \
    's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd \
    "${_pkg}"
  # Drop dep on libatomic
  # https://gitlab.archlinux.org/archlinux/packaging/packages/qemu/-/issues/6
  git \
    revert \
      -n \
        "4e6dc4dee0e1c6407113597180d9616b4f275f94"
  # Suppress noise from glib-compile-schemas.hook
  git \
    apply \
      -3 \
      "../0001-${_pkg}-compile-schemas-Remove-noisy-deprecation-warnin.patch"
  git \
    submodule \
      init
  git \
    submodule \
      set-url \
        subprojects/gvdb \
        "${srcdir}/gvdb"
  git \
    -c \
      protocol.file.allow=always \
    submodule \
      update
}

_prefix="$( \
  dirname \
    "$(dirname \
        "$(command \
             -v \
             "cc" \
             "clang" \
             "gcc" | \
             head \
               -n \
                 1)")")"

if [[ "${_docs}" == "false" ]]; then
  _man_pages="disabled"
fi

meson_options=(
  --default-library both
  -D glib_debug="${_glib_debug}"
  # -D gtk_doc="${_docs}"
  -D documentation="${_docs}"
  # -D introspection="true"
  # -D man="${_docs}"
  -D man-pages="${_man_pages}"
  -D glib_checks="${_checks}"
  -D libmount="${_libmount}"
  -D selinux=disabled
  -D sysprof="${_sysprof}"
  -D tests="${_checks}"
  -D systemtap="${_systemtap}"
  -D dtrace="${_dtrace}"
)

build() {
  # Produce more debug info: GLib has a lot of useful macros
  CFLAGS+=" -g3"
  CXXFLAGS+=" -g3"
  if [[ "${_os}" == "Android" ]]; then
    CFLAGS+=" -Wno-error=format-security"
    CFLAGS+=" -Wno-error=format-nonliteral"
    CFLAGS+=" -Wno-error=implicit-function-declaration"
  # use fat LTO objects for static libraries
    CFLAGS+=" -ffat-lto-objects"
    CXXFLAGS+=" -ffat-lto-objects"
  fi
  arch-meson \
    "${_pkg}" \
      build \
        "${meson_options[@]}"
  meson \
    compile \
      -C \
        build
}

check() {
  meson \
    test \
    -C \
      build \
        --no-suite \
          flaky \
        --no-suite \
          slow \
        --print-errorlogs
}

_pick() {
  local \
    _p="${1}" \
    _f \
    _d
  shift
  for _f; do
    _d="${srcdir}/${_p}/${_f#${pkgdir}/}"
    mkdir \
      -p \
      "$(dirname \
           "${_d}")"
    mv \
      "${_f}" \
      "${_d}"
    rmdir \
      -p \
      --ignore-fail-on-non-empty \
      "$(dirname \
           "${_f}")"
    done
}


package_glib2() {
  local \
    _f
  if [[ "${_os}" == "GNU/Linux" ]]; then
    depends+=(
      "libffi.so"
    )
  elif [[ "${_os}" == "Android" ]]; then
    depends+=(
      "libffi"
    )
  fi
  if [[ "${_libmount}" == "enabled" ]]; then
    depends+=(
      "libmount.so"
    )
  fi
  # But really, glib 1.x builds fine so
  # termux-pacman should rename glib to glib2
  if [[ "${_os}" == "Android" ]]; then
    provides+=(
      "${_pkg}=${pkgver}"
      "${_pkg}-bin=${pkgver}"
      "libg"{"lib","io","module","object","thread"}"-2.0.so=${pkgver}"
    )
    # termux should package glib1
    # conflicts+=(
    #   "${_pkg}"
    #   "${_pkg}-bin"
    # )
  fi
  optdepends=(
    'dconf: GSettings storage backend'
    'gvfs: most gio functionality'
    'libelf: gresource inspection tool'
  )
  meson \
    install \
      -C \
        build \
      --destdir \
        "${pkgdir}"
  install \
    -Dt \
    "${pkgdir}/usr/share/libalpm/hooks" \
    -m644 \
    *.hook
  touch \
    "${pkgdir}/usr/lib/gio/modules/.keep"
  "${_py}" \
    -m compileall \
    -d "/usr/share/${_pkg}-2.0/codegen" \
    "$pkgdir/usr/share/${_pkg}-2.0/codegen"
  "${_py}" \
    -O \
    -m compileall \
    -d "/usr/share/${_pkg}-2.0/codegen" \
    "${pkgdir}/usr/share/${_pkg}-2.0/codegen"
  for _f  \
    in $(find \
          "${pkgdir}/usr/lib/pkgconfig/" | \
          grep ".pc"); do
    sed \
      -i \
      "s%prefix=/usr%prefix=${_prefix}%" \
      "${_f}"
  done
  cd \
    "${pkgdir}"
  # Split docs
  if [[ "${_docs}" == true ]]; then
  _pick \
    "docs" \
    "usr/share/doc"
  fi
  # Split devel
  _pick \
    "devel" \
    "usr/bin/gdbus-codegen"
  _pick \
    "devel" \
    "usr/bin/glib-"{"mkenums","genmarshal"}
  _pick \
    "devel" \
    "usr/bin/gresource"
  _pick \
    "devel" \
    "usr/bin/gtester"{"","-report"}
  _pick \
    "devel" \
    "usr/share/gdb/"
  _pick \
    "devel" \
    "usr/share/glib-2.0/gdb/"
  _pick \
    "devel" \
    "usr/share/glib-2.0/codegen/"
  if [[ "${_bash_completion}" == "true" ]]; then
    _pick \
      "devel" \
      "usr/share/bash-completion/completions/gresource"
  fi
  if [[ "${_docs}" == "true" ]]; then
    _pick \
      "devel" \
      "usr/share/man/man1/gdbus-codegen.1"
    _pick \
      "devel" \
      "usr/share/man/man1/glib-"{"mkenums","genmarshal"}".1"
    _pick \
      "devel" \
      "usr/share/man/man1/gresource.1"
    _pick \
      "devel" \
      "usr/share/man/man1/gtester"{"","-report"}".1"
  fi
}

package_glib2-devel() {
  pkgdesc+=" - development files"
  depends=(
    "${_pkg}2"
    "${_libc}"
    "libelf"
    "${_py}"
    "${_py}-packaging"
  )
  mv \
    "devel/"* \
    "${pkgdir}"
}


package_glib2-docs() {
  pkgdesc+=" - documentation"
  depends=()
  license+=(
    'LicenseRef-Public-Domain'
  )
  mv \
    "docs/"* \
    "${pkgdir}"
  install \
    -Dt \
    "${pkgdir}/usr/share/licenses/${pkgname}" \
    -m644 \
    "${_pkg}/docs/reference/COPYING"
}

# vim:set sw=2 sts=-1 et:
