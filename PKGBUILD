# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.26.0
pkgrel=2
pkgdesc="Common C routines used by GTK+ 2.4 and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=8.02')
makedepends=('pkgconfig' 'python2')
options=('!libtool' '!docs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.26/glib-${pkgver}.tar.bz2
        glib2.sh
        glib2.csh)
sha256sums=('4c18e3aadb5b20acc7c0f7d3a77da8a2843b85a9fd73fd3aa360a7aea953e3b2'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3')

build() {
  cd "${srcdir}/glib-${pkgver}"
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
