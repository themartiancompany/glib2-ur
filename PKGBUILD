# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.52.2+1+gb8bd46bc8
pkgrel=1
pkgdesc="Low level core library"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=(gettext gtk-doc libffi pcre zlib shared-mime-info python libelf git util-linux)
checkdepends=(desktop-file-utils dbus)
_commit=b8bd46bc862598db41013f4b56b0d74126e25c52  # glib-2-52
source=("git+https://git.gnome.org/browse/glib#commit=$_commit"
        glib-compile-schemas.hook
        gio-querymodules.hook
        skip-broken-timer-test.patch
        bad-dconf-paths.patch)
sha256sums=('SKIP'
            'e1123a5d85d2445faac33f6dae1085fdd620d83279a4e130a83fe38db52b62b3'
            '5ba204a2686304b1454d401a39a9d27d09dd25e4529664e3fd565be3d439f8b6'
            'a39dc8c1c1707053d565d5b198b1f03f7c55b31e11335a1bfdc3f9803b114d5d'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

pkgver() {
  cd glib
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd glib
  git revert -n e4ce400e8f7f  # https://bugzilla.gnome.org/show_bug.cgi?id=781601
  git cherry-pick -n 9ba95e25b74a  # https://bugs.archlinux.org/task/53730

  # Rounding error in timer tests?
  # GLib:ERROR:timer.c:38:test_timer_basic: assertion failed (micros == ((guint64)(elapsed * 1e6)) % 1000000): (1 == 0)
  [[ $CARCH == i686 ]] && patch -Np1 -i ../skip-broken-timer-test.patch

  patch -Rp1 -i ../bad-dconf-paths.patch
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd glib
  ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam \
      --enable-gtk-doc \
      $(check_option debug y && echo --enable-debug=yes)
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

check() {
  cd glib
  make check
}

package_glib2() {
  depends=(pcre libffi libutil-linux)
  optdepends=('python: for gdbus-codegen and gtester-report'
              'libelf: gresource inspection tool')
  options=(!emptydirs)
  license=(LGPL)

  cd glib
  make DESTDIR="$pkgdir" install
  rm -r "$pkgdir/usr/share/gtk-doc"

  # install hooks
  install -d "$pkgdir/usr/share/libalpm/hooks/"
  install -m644 "$srcdir"/{glib-compile-schemas,gio-querymodules}.hook \
    "$pkgdir/usr/share/libalpm/hooks/"
}

package_glib2-docs() {
  pkgdesc="Documentation for glib2"
  conflicts=(gobject2-docs)
  replaces=(gobject2-docs)
  license=(custom)
  options=(!emptydirs)

  cd glib/docs
  make DESTDIR="$pkgdir" install
  rm -r "$pkgdir/usr/share/man"

  install -Dm644 reference/COPYING "$pkgdir/usr/share/licenses/glib2-docs/COPYING"
}
