# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.36.0
pkgrel=2
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre' 'libffi')
makedepends=('pkg-config' 'python2' 'libxslt' 'docbook-xml')
optdepends=('python2: for gdbus-codegen and gtester-report')
options=('!libtool' '!docs' '!emptydirs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver%.*}/glib-$pkgver.tar.xz
        revert-warn-glib-compile-schemas.patch)
sha256sums=('455a8abe8692c5174bcc7ffa15b96a7521a2f2f9fb47594405927c35cb9bb227'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

build() {
  cd glib-$pkgver
  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
  PYTHON=python2 ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

package() {
  cd glib-$pkgver
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  for _i in "$pkgdir/usr/share/bash-completion/completions/"*; do
      chmod -x "$_i"
  done

   sed -i "s|#!/usr/bin/env python|#!/usr/bin/env python2|" "$pkgdir"/usr/bin/gdbus-codegen
}
