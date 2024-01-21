# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Pellegrino Prevete <pellegrinoprevetea@gmail.com>
# Contributor: Truocolo <truocolo@aol.com>

_docs=false
_py="python"
_pkg="glib"
_proj="gnome"
_ns="GNOME"
pkgbase="${_pkg}2"
pkgname=(
  "${pkgbase}"
)
[[ "${_docs}" == "true" ]] && \
  pkgname+=(
    "${pkgbase}-docs"
  )
pkgver=2.78.3
pkgrel=1
pkgdesc="Low level core library"
_http="http://gitlab.${_proj}.org"
url="${_http}/${_ns}/${_pkg}"
_local="file://${HOME}/${_pkg}"
license=(
  LGPL
)
arch=(
  x86_64
  arm
  aarch64
  powerpc
  i686
  pentium4
)
depends=(
  libffi
  libsysprof-capture
  pcre2
  util-linux-libs
  zlib
)
makedepends=(
  dbus
  gettext
  git
  gtk-doc
  libelf
  meson
  "${_py}"
  shared-mime-info
  util-linux
)
checkdepends=(
  desktop-file-utils
  "${_pkg}2"
)
options=(
  debug
  staticlibs
)
_commit=03f7c1fbf3a3784cb4c3604f83ca3645e9225577  # tags/2.78.3^0
source=(
  # "git+${url}.git#commit=$_commit"
  "git+${_local}#commit=$_commit"
  "git+${_http}/${_ns}/gvdb.git"
  0001-glib-compile-schemas-Remove-noisy-deprecation-warnin.patch
  gio-querymodules.hook
  glib-compile-schemas.hook
)
b2sums=('SKIP'
        'SKIP'
        '94c73ca7070c239494873dd52d6ee09382bbb5b1201f7afd737cfa140b1a2fb0744b2c2831baf3943d1d072550c35888d21ce6f19f89481ff9d1a60d9a0b30e0'
        '14c9211c0557f6d8d9a914f1b18b7e0e23f79f4abde117cb03ab119b95bf9fa9d7a712aa0a29beb266468aeb352caa3a9e4540503cfc9fe0bbaf764371832a96'
        'd30d349b4cb4407839d9074ce08f5259b8a5f3ca46769aabc621f17d15effdb89c4bf19bd23603f6df3d59f8d1adaded0f4bacd0333afcab782f2d048c882858')


pkgver() {
  cd glib
  git describe --tags | sed 's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd glib

  # Suppress noise from glib-compile-schemas.hook
  git apply -3 ../0001-glib-compile-schemas-Remove-noisy-deprecation-warnin.patch

  git submodule init
  git submodule set-url subprojects/gvdb "$srcdir/gvdb"
  git -c protocol.file.allow=always submodule update
}

meson_options=(
  --default-library both
  -D glib_debug=disabled
  -D gtk_doc=${_docs}
  -D man=true
  -D selinux=disabled
  -D sysprof=disabled
)

build() {
  # Produce more debug info: GLib has a lot of useful macros
  CFLAGS+=" -g3"
  CXXFLAGS+=" -g3"
  # use fat LTO objects for static libraries
  CFLAGS+=" -ffat-lto-objects"
  CXXFLAGS+=" -ffat-lto-objects"
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

package_glib2() {
  depends+=(
    libffi.so
    libmount.so
  )
  provides+=(
    "${_pkg}=${pkgver}"
    "libg"{lib,io,module,object,thread}"-2.0.so=${pkgver}"
  )
  optdepends=(
    'gvfs: most gio functionality'
    'libelf: gresource inspection tool'
    "${_py}: gdbus-codegen, glib-genmarshal, glib-mkenums, gtester-report"
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

  # Split docs
  if \
    [[ "${_docs}" == true ]]; then
    mkdir \
      -p \
      docs/usr/share
    mv \
      {"${pkgdir}",docs}"/usr/share/gtk-doc"
  fi
}

package_glib2-docs() {
  pkgdesc+=" - documentation"
  depends=()
  license+=(
  custom
)

  mv \
    -t \
      "${pkgdir}" \
    "docs/"*
  install \
    -Dt \
      "${pkgdir}/usr/share/licenses/${pkgname}" \
      -m644 \
        "${_pkg}/docs/reference/COPYING"
}

# vim:set sw=2 sts=-1 et:
