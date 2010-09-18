# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.25.16
pkgrel=1
pkgdesc="Common C routines used by GTK+ 2.4 and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=8.02')
makedepends=('pkgconfig')
options=('!libtool' '!docs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.25/glib-${pkgver}.tar.bz2
        glib2.sh
        glib2.csh revert-redundant-headers.patch)
sha256sums=('dd7243298504792ab717cea138554ab7edb8ec4145f6b9b0839ffe3ae0ad39f2'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3'
            '4a40619f3d3eef6accf22c0da67b73632688c7a8e8b350be1acce8a10d1befe4')

build() {
  cd "${srcdir}/glib-${pkgver}"
  patch -NRp1 -i "${srcdir}/revert-redundant-headers.patch"
  ./configure --prefix=/usr \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
  make DESTDIR="${pkgdir}" install

  install -d "${pkgdir}/etc/profile.d"
  install -m755 "${srcdir}/glib2.sh" "${pkgdir}/etc/profile.d/"
  install -m755 "${srcdir}/glib2.csh" "${pkgdir}/etc/profile.d/"
}
