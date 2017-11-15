# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.54.2
pkgrel=2
pkgdesc="Low level core library"
url="https://wiki.gnome.org/Projects/GLib"
license=(LGPL2.1)
arch=(x86_64)
depends=(pcre libffi libutil-linux zlib)
makedepends=(gettext gtk-doc shared-mime-info python libelf git util-linux meson dbus)
checkdepends=(desktop-file-utils)
optdepends=('python: for gdbus-codegen and gtester-report'
            'libelf: gresource inspection tool')
options=(!emptydirs)
_commit=52b3e434d849e2b0233d6d3d663b5dda82ab263e  # tags/2.54.2^0
source=("git+https://git.gnome.org/browse/glib#commit=$_commit"
        0001-docs-Fix-building-with-meson.patch
        0001-meson-Fix-permissions-of-installed-scripts.patch
        0001-meson-Fix-GDB-scripts-install_dir-for-nix.patch
        0001-meson-Fix-libmount-support.patch
        libs.diff
        noisy-glib-compile-schemas.diff
        glib-compile-schemas.hook gio-querymodules.hook)
sha256sums=('SKIP'
            '8b289f3e1a5a3b29d310d45610468199acfe6f2b38a0d1be38c9224437a0e40c'
            '12b1a2f4e304e4c03e48ae9564d73ae38619bbb7711a013138939ff8e5cc2327'
            'f53d5acfda4b7141a4813f1e49610e9176dc5bdf8e867d88290e34d91a40ebcb'
            '14c0dcfb4efb690f7cb44673faf31f5eb40ac68c01974eddb53d5aa7ae366629'
            '2fb828f51727bd9c8b48cfd9d6833c8b4ff82803331f6e2340b0ec8edfe57c52'
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

  # https://bugzilla.gnome.org/show_bug.cgi?id=788772
  patch -Np1 -i ../0001-meson-Fix-GDB-scripts-install_dir-for-nix.patch

  # https://bugzilla.gnome.org/show_bug.cgi?id=789681
  patch -Np1 -i ../0001-meson-Fix-libmount-support.patch

  # https://bugzilla.gnome.org/show_bug.cgi?id=788773
  patch -Np1 -i ../libs.diff

  # Suppress noise from glib-compile-schemas.hook
  patch -Np1 -i ../noisy-glib-compile-schemas.diff
}

build() {
  cd build
  arch-meson ../glib
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
