# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.40.0
pkgrel=2
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=('pkg-config' 'python2' 'libxslt' 'docbook-xml' 'pcre' 'libffi' 'elfutils' 'gtk-doc' 'git')
source=('git://git.gnome.org/glib#commit=938a468acf58499b7347fa923384829d488b0ef6'
        revert-warn-glib-compile-schemas.patch)
sha256sums=('SKIP'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

prepare() {
  cd glib
  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
}
  
build() {
  cd glib
  NOCONFIGURE=1 ./autogen.sh
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam \
      --enable-gtk-doc
  make
}

check() {
  cd glib
  #make -k check || :
}

package_glib2() {
  depends=('pcre' 'libffi')
  optdepends=('python2: for gdbus-codegen and gtester-report'
              'elfutils: gresource inspection tool')
  options=('!docs' '!emptydirs')
  license=('LGPL')

  cd glib
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  for _i in "$pkgdir/usr/share/bash-completion/completions/"*; do
      chmod -x "$_i"
  done

  # Our gdb does not ship the required python modules, so remove it
  rm -rf "$pkgdir/usr/share/gdb/"
}

package_glib2-docs() {
  pkgdesc="Documentation for glib2"
  conflicts=('gobject2-docs')
  replaces=('gobject2-docs')
  license=('custom')
  options=('docs' '!emptydirs')
  
  cd glib/docs
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/usr/share/licenses/glib2-docs"
  install -m644 reference/COPYING "${pkgdir}/usr/share/licenses/glib2-docs/"

  rm -rf "${pkgdir}/usr/share/man"
}
