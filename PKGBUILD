# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.54.0+15+g20364bbdf
pkgrel=1
pkgdesc="Low level core library"
url="https://wiki.gnome.org/Projects/GLib"
license=(LGPL2.1)
arch=(i686 x86_64)
depends=(pcre libffi libutil-linux)
makedepends=(gettext gtk-doc zlib shared-mime-info python libelf git util-linux meson)
checkdepends=(desktop-file-utils dbus)
optdepends=('python: for gdbus-codegen and gtester-report'
            'libelf: gresource inspection tool')
options=(!emptydirs)
_commit=20364bbdf55a7662b657ad50e16c6b121463809b  # glib-2-54
source=("git+https://git.gnome.org/browse/glib#commit=$_commit"
        0001-docs-Fix-building-with-meson.patch
        0001-meson-Fix-permissions-of-installed-scripts.patch
        libs.diff
        noisy-glib-compile-schemas.diff
        glib-compile-schemas.hook gio-querymodules.hook)
sha256sums=('SKIP'
            '8b289f3e1a5a3b29d310d45610468199acfe6f2b38a0d1be38c9224437a0e40c'
            '12b1a2f4e304e4c03e48ae9564d73ae38619bbb7711a013138939ff8e5cc2327'
            '05cce54fa4a728e2f0db45326ba3d4f303c2cc4d0cc68506e338906ad17ea7f0'
            '81a4df0b638730cffb7fa263c04841f7ca6b9c9578ee5045db6f30ff0c3fc531'
            'e1123a5d85d2445faac33f6dae1085fdd620d83279a4e130a83fe38db52b62b3'
            '5ba204a2686304b1454d401a39a9d27d09dd25e4529664e3fd565be3d439f8b6')

pkgver() {
  cd glib
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  mkdir -p build glib2-docs/usr/share
  cd glib

  # https://bugzilla.gnome.org/show_bug.cgi?id=786796
  patch -Np1 -i ../0001-docs-Fix-building-with-meson.patch

  # https://bugzilla.gnome.org/show_bug.cgi?id=787671
  patch -Np1 -i ../0001-meson-Fix-permissions-of-installed-scripts.patch

  # Unbreak .pc files when built with meson
  # Fix gdb scripts install
  patch -Np1 -i ../libs.diff

  # Suppress noise from glib-compile-schemas.hook
  patch -Np1 -i ../noisy-glib-compile-schemas.diff
}

build() {
  cd build
  meson setup --prefix=/usr --buildtype=release ../glib
  ninja
}

check() {
  cd build
  meson test -t 2
}

package_glib2() {
  cd build
  DESTDIR="$pkgdir" ninja install
  mv "$pkgdir/usr/share/gtk-doc" "$srcdir/glib2-docs/usr/share"

  # install hooks
  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 ../*.hook
}

package_glib2-docs() {
  pkgdesc="Documentation for GLib"
  depends=()
  optdepends=()
  license+=(custom)

  mv glib2-docs/* "$pkgdir"
  install -Dt "$pkgdir/usr/share/licenses/glib2-docs" -m644 glib/docs/reference/COPYING
}
