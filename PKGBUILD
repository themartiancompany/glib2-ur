# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.58.1+67+g17519e039
pkgrel=1
pkgdesc="Low level core library"
url="https://wiki.gnome.org/Projects/GLib"
license=(LGPL2.1)
arch=(x86_64)
depends=(pcre libffi libutil-linux zlib)
makedepends=(gettext gtk-doc shared-mime-info python libelf git util-linux meson dbus)
checkdepends=(desktop-file-utils)
optdepends=('python: gdbus-codegen, glib-genmarshal, glib-mkenums, gtester-report'
            'libelf: gresource inspection tool')
options=(!emptydirs)
_commit=17519e039f29b5ffd0aad5ed1661bc56eb1dfeaa  # glib-2-58
source=("git+https://gitlab.gnome.org/GNOME/glib.git#commit=$_commit"
        noisy-glib-compile-schemas.diff
        glib-compile-schemas.hook gio-querymodules.hook)
sha256sums=('SKIP'
            '81a4df0b638730cffb7fa263c04841f7ca6b9c9578ee5045db6f30ff0c3fc531'
            'e1123a5d85d2445faac33f6dae1085fdd620d83279a4e130a83fe38db52b62b3'
            '5ba204a2686304b1454d401a39a9d27d09dd25e4529664e3fd565be3d439f8b6')

pkgver() {
  cd glib
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd glib

  # Suppress noise from glib-compile-schemas.hook
  patch -Np1 -i ../noisy-glib-compile-schemas.diff
}

build() {
  arch-meson glib build \
    -D selinux=false \
    -D man=true \
    -D gtk_doc=true
  ninja -C build
}

check() {
  meson test -C build
}

package_glib2() {
  DESTDIR="$pkgdir" meson install -C build
  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 *.hook

  python -m compileall -d /usr/share/glib-2.0/codegen "$pkgdir/usr/share/glib-2.0/codegen"
  python -O -m compileall -d /usr/share/glib-2.0/codegen "$pkgdir/usr/share/glib-2.0/codegen"

  # Split docs
  mv "$pkgdir/usr/share/gtk-doc" "$srcdir"
}

package_glib2-docs() {
  pkgdesc="Documentation for GLib"
  depends=()
  optdepends=()
  license+=(custom)

  mkdir -p "$pkgdir/usr/share"
  mv gtk-doc "$pkgdir/usr/share"

  install -Dt "$pkgdir/usr/share/licenses/glib2-docs" -m644 glib/docs/reference/COPYING
}

# vim:set sw=2 et:
