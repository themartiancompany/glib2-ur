# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.32.4
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre' 'libffi')
makedepends=('pkg-config' 'python2')
optdepends=('python2: for gdbus-codegen')
options=('!libtool' '!docs' '!emptydirs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver%.*}/glib-$pkgver.tar.xz
        glib2.sh
        glib2.csh
        revert-warn-glib-compile-schemas.patch)
sha256sums=('a5d742a4fda22fb6975a8c0cfcd2499dd1c809b8afd4ef709bda4d11b167fae2'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

build() {
  cd glib-$pkgver
  patch -Rp1 -i "$srcdir/revert-warn-glib-compile-schemas.patch"
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

package() {
  cd glib-$pkgver
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  install -d "$pkgdir/etc/profile.d"
  install -m755 "$srcdir/glib2.sh" "$pkgdir/etc/profile.d/"
  install -m755 "$srcdir/glib2.csh" "$pkgdir/etc/profile.d/"

  for _i in "$pkgdir/usr/share/bash-completion/completions/"*; do
      chmod -x "$_i"
  done
  sed -i "s|#!/usr/bin/env python|#!/usr/bin/env python2|" "$pkgdir"/usr/bin/gdbus-codegen
}
