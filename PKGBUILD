# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.28.5
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=8.02')
makedepends=('pkgconfig' 'python2')
options=('!libtool' '!docs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.28/glib-${pkgver}.tar.bz2
        glib2.sh
        glib2.csh)
sha256sums=('8eb4b56b228c6d0bf5021dd23db5b0084d80cc6d8d89d7863073c2da575ec22a'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3')

build() {
  cd "${srcdir}/glib-${pkgver}"
  ./configure --prefix=/usr \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

package() {
  cd "${srcdir}/glib-${pkgver}"
  make DESTDIR="${pkgdir}" install

  install -d "${pkgdir}/etc/profile.d"
  install -m755 "${srcdir}/glib2.sh" "${pkgdir}/etc/profile.d/"
  install -m755 "${srcdir}/glib2.csh" "${pkgdir}/etc/profile.d/"

  for _i in "${pkgdir}/etc/bash_completion.d/"*; do
      chmod -x "${_i}"
  done
}
