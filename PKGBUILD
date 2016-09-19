# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.49.7+6+g3602f93
pkgrel=1
pkgdesc="Low level core library"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=('gettext' 'gtk-doc' 'libffi' 'pcre' 'zlib' 'shared-mime-info' 'python' 'libelf' 'git')
_commit=3602f934855a484c5eec28f12a6535e14de1778d
source=("git://git.gnome.org/glib#commit=$_commit"
        glib-compile-schemas.hook
        gio-querymodules.hook
        revert-warn-glib-compile-schemas.patch)
sha256sums=('SKIP'
            'e1123a5d85d2445faac33f6dae1085fdd620d83279a4e130a83fe38db52b62b3'
            '5ba204a2686304b1454d401a39a9d27d09dd25e4529664e3fd565be3d439f8b6'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

pkgver() {
  cd glib
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd glib
  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
  NOCONFIGURE=1 ./autogen.sh
}
  
build() {
  cd glib
  ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam \
      --enable-gtk-doc
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

check() {
  cd glib
  # Takes an effing long time
  #make -k check || :
}

package_glib2() {
  depends=('pcre' 'libffi')
  optdepends=('python: for gdbus-codegen and gtester-report'
              'libelf: gresource inspection tool')
  options=('!docs' '!emptydirs')
  license=('LGPL')

  cd glib
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  chmod -x "$pkgdir"/usr/share/bash-completion/completions/*

  # install hooks
  install -dm755 "$pkgdir"/usr/share/libalpm/hooks/
  install -m644 "$srcdir"/{glib-compile-schemas,gio-querymodules}.hook "$pkgdir"/usr/share/libalpm/hooks/
}

package_glib2-docs() {
  pkgdesc="Documentation for glib2"
  conflicts=('gobject2-docs')
  replaces=('gobject2-docs')
  license=('custom')
  options=('docs' '!emptydirs')
  
  cd glib/docs
  make DESTDIR="${pkgdir}" install
  rm -rf "${pkgdir}/usr/share/man"
  install -m755 -d "${pkgdir}/usr/share/licenses/glib2-docs"
  install -m644 reference/COPYING "${pkgdir}/usr/share/licenses/glib2-docs/"
}
