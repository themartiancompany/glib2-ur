# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.36.3
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=('pkg-config' 'python2' 'libxslt' 'docbook-xml' 'pcre' 'libffi' 'elfutils')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver%.*}/glib-$pkgver.tar.xz
        revert-warn-glib-compile-schemas.patch)
sha256sums=('5ec433bf6ce02e4c436619c3d0b9cecdd1898469398a636bad27c1f5804c761e'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

build() {
  cd glib-$pkgver

  # workaround FS#34630 https://bugzilla.gnome.org/show_bug.cgi?id=698716
  export CFLAGS+=" -Wall"

  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

check() {
  cd glib-$pkgver
  #make -k check || :
}

package_glib2() {
  depends=('pcre' 'libffi')
  optdepends=('python2: for gdbus-codegen and gtester-report'
              'elfutils: gresource inspection tool')
  options=('!docs' '!libtool' '!emptydirs')
  license=('LGPL')

  cd glib-$pkgver
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
  
  cd glib-$pkgver/docs
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/usr/share/licenses/glib2-docs"
  install -m644 reference/COPYING "${pkgdir}/usr/share/licenses/glib2-docs/"

  rm -rf "${pkgdir}/usr/share/man"
}
