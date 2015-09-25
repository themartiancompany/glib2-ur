# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.46.0
pkgrel=2
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=('pkg-config' 'python2' 'libxslt' 'docbook-xml' 'pcre' 'libffi' 'libelf')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver:0:4}/glib-$pkgver.tar.xz
        0001-Revert-list-store-Fix-a-parameter-check.patch
        revert-warn-glib-compile-schemas.patch)
sha256sums=('b1cee83469ae7d80f17c267c37f090414e93960bd62d2b254a5a96fbc5baacb4'
            '261ae2d2c7b94460f33ab569540313e21c9a50af38a7ebe8412e49f5b309af35'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

prepare() {
  cd glib-$pkgver
  patch -Np1 -i ../0001-Revert-list-store-Fix-a-parameter-check.patch
  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
}
  
build() {
  cd glib-$pkgver
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
              'libelf: gresource inspection tool')
  options=('!docs' '!emptydirs')
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
