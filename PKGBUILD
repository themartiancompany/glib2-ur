# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib
pkgname=(glib glib-docs)
pkgver=2.53.7
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
provides=("glib2=$pkgver")
conflicts=(glib2)
replaces=(glib2)
options=(!emptydirs)
_commit=052f134528ae5bf828f39684efe2ff4d4e0cf24c  # tags/2.53.7^0
source=("git+https://git.gnome.org/browse/glib#commit=$_commit"
        0001-docs-Fix-building-with-meson.patch
        noisy-glib-compile-schemas.diff
        glib-compile-schemas.hook gio-querymodules.hook)
sha256sums=('SKIP'
            '8b289f3e1a5a3b29d310d45610468199acfe6f2b38a0d1be38c9224437a0e40c'
            '81a4df0b638730cffb7fa263c04841f7ca6b9c9578ee5045db6f30ff0c3fc531'
            'e1123a5d85d2445faac33f6dae1085fdd620d83279a4e130a83fe38db52b62b3'
            '5ba204a2686304b1454d401a39a9d27d09dd25e4529664e3fd565be3d439f8b6')

pkgver() {
  cd glib
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  mkdir -p build glib-docs/usr/share
  cd glib

  # https://bugzilla.gnome.org/show_bug.cgi?id=786796
  patch -Np1 -i ../0001-docs-Fix-building-with-meson.patch

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
  meson test
}

package_glib() {
  cd build
  DESTDIR="$pkgdir" ninja install
  mv "$pkgdir/usr/share/gtk-doc" "$srcdir/glib-docs/usr/share"

  # install hooks
  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 ../*.hook
}

package_glib-docs() {
  pkgdesc="Documentation for glib"
  depends=()
  optdepends=()
  provides=("glib2-docs=$pkgver")
  conflicts=(glib2-docs)
  replaces=(glib2-docs)
  license+=(custom)

  mv glib-docs/* "$pkgdir"
  install -Dt "$pkgdir/usr/share/licenses/glib-docs" -m644 glib/docs/reference/COPYING
}
