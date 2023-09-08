# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(
  glib2
  glib2-docs
)
pkgver=2.76.5
pkgrel=2
pkgdesc="Low level core library"
url="https://wiki.gnome.org/Projects/GLib"
license=(LGPL)
arch=(x86_64)
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
  python
  shared-mime-info
  util-linux
)
checkdepends=(
  desktop-file-utils
  glib2
)
options=(
  debug
  staticlibs
)
_commit=f0171c9eccdf9ebeabb074d4683fc9cfc41f4e60  # tags/2.76.5^0
source=(
  "git+https://gitlab.gnome.org/GNOME/glib.git#commit=$_commit"
  "git+https://gitlab.gnome.org/GNOME/gvdb.git"
  0001-glib-compile-schemas-Remove-noisy-deprecation-warnin.patch
  0002-glocalfile-Sum-apparent-size-only-for-files-and-syml.patch
  0003-tests-file-Do-not-rely-on-du-bytes-behaviour.patch
  gio-querymodules.hook
  glib-compile-schemas.hook
)
b2sums=('SKIP'
        'SKIP'
        '94c73ca7070c239494873dd52d6ee09382bbb5b1201f7afd737cfa140b1a2fb0744b2c2831baf3943d1d072550c35888d21ce6f19f89481ff9d1a60d9a0b30e0'
        '6bcbcba60208162f7221701d6a642eabfc92c2fc6a476bcb42da5967577f8f0c75b688d149be01c9c48cd644aafa7fbdd63d9086385b8f7607fc981756d71a68'
        '257bf37d304cc161dedcde0a2c4d01e297f8263cde48b49d3ee47ca95a8fb9ad44bbb9bf99da51ec766ffb6f9d502e0a8fdc6b86346e6755373ee515e23b9419'
        '14c9211c0557f6d8d9a914f1b18b7e0e23f79f4abde117cb03ab119b95bf9fa9d7a712aa0a29beb266468aeb352caa3a9e4540503cfc9fe0bbaf764371832a96'
        'd30d349b4cb4407839d9074ce08f5259b8a5f3ca46769aabc621f17d15effdb89c4bf19bd23603f6df3d59f8d1adaded0f4bacd0333afcab782f2d048c882858')


pkgver() {
  cd glib
  git describe --tags | sed 's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd glib

  # Unbreak keyfiles with bad escapes
  # https://bugs.archlinux.org/task/79540
  # https://gitlab.gnome.org/GNOME/glib/-/merge_requests/3565
  git cherry-pick -n 4a9672764214d5fab569b774fe761ae7d2ec11d9

  # Suppress noise from glib-compile-schemas.hook
  git apply -3 ../0001-glib-compile-schemas-Remove-noisy-deprecation-warnin.patch

  # fix test suite issues with coreutils >=9.2
  # https://gitlab.gnome.org/GNOME/glib/-/merge_requests/3358
  git apply -3 ../0002-glocalfile-Sum-apparent-size-only-for-files-and-syml.patch
  git apply -3 ../0003-tests-file-Do-not-rely-on-du-bytes-behaviour.patch

  git submodule init
  git submodule set-url subprojects/gvdb "$srcdir/gvdb"
  git -c protocol.file.allow=always submodule update
}

build() {
  local meson_options=(
    --default-library both
    -D glib_debug=disabled
    -D gtk_doc=true
    -D man=true
    -D selinux=disabled
    -D sysprof=enabled
  )

  # Produce more debug info: GLib has a lot of useful macros
  CFLAGS+=" -g3"
  CXXFLAGS+=" -g3"

  # use fat LTO objects for static libraries
  CFLAGS+=" -ffat-lto-objects"
  CXXFLAGS+=" -ffat-lto-objects"

  arch-meson glib build "${meson_options[@]}"
  meson compile -C build
}

check() {
  meson test -C build --no-suite flaky --no-suite slow --print-errorlogs
}

package_glib2() {
  depends+=(
    libffi.so
    libmount.so
  )
  provides+=(libg{lib,io,module,object,thread}-2.0.so)
  optdepends=(
    'gvfs: most gio functionality'
    'libelf: gresource inspection tool'
    'python: gdbus-codegen, glib-genmarshal, glib-mkenums, gtester-report'
  )

  meson install -C build --destdir "$pkgdir"

  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 *.hook
  touch "$pkgdir/usr/lib/gio/modules/.keep"

  python -m compileall -d /usr/share/glib-2.0/codegen \
    "$pkgdir/usr/share/glib-2.0/codegen"
  python -O -m compileall -d /usr/share/glib-2.0/codegen \
    "$pkgdir/usr/share/glib-2.0/codegen"

  # Split docs
  mkdir -p docs/usr/share
  mv {"$pkgdir",docs}/usr/share/gtk-doc
}

package_glib2-docs() {
  pkgdesc+=" - documentation"
  depends=()
  license+=(custom)

  mv -t "$pkgdir" docs/*
  install -Dt "$pkgdir/usr/share/licenses/$pkgname" -m644 glib/docs/reference/COPYING
}

# vim:set sw=2 sts=-1 et:
