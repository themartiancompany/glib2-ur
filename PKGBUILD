# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.31.20
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre' 'libffi')
makedepends=('pkgconfig' 'python2')
optdepends=('python2: for gdbus-codegen')
options=('!libtool' '!docs' '!emptydirs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver%.*}/glib-$pkgver.tar.xz
        glib2.sh
        glib2.csh)
sha256sums=('3475e1d866c462a36b89d4bae91181513c390ad0af25f445618321da1e022c2a'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3')

build() {
  cd "$srcdir/glib-$pkgver"
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

package() {
  cd "$srcdir/glib-$pkgver"
  make DESTDIR="$pkgdir" install

  install -d "$pkgdir/etc/profile.d"
  install -m755 "$srcdir/glib2.sh" "$pkgdir/etc/profile.d/"
  install -m755 "$srcdir/glib2.csh" "$pkgdir/etc/profile.d/"

  for _i in "$pkgdir/etc/bash_completion.d/"*; do
      chmod -x "$_i"
  done
  sed -i "s|#!/usr/bin/env python|#!/usr/bin/env python2|" "$pkgdir"/usr/bin/gdbus-codegen
}
