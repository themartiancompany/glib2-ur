# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.24.1
pkgrel=1
pkgdesc="Common C routines used by GTK+ 2.4 and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=8.02')
makedepends=('pkgconfig')
options=('!libtool' '!docs')
conflicts=('glib2')
provides=('glib2')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.24/glib-${pkgver}.tar.bz2
        glib2.sh
        glib2.csh)
sha1sums=('d4835bb1618fc1e1dfe88ef8443c12fcae69f90e'
          'bfe05590a6498259f1045a591fd886a8572f271a'
          '6db09da816d69aab7a5cbf3460ee082bef200891')

build() {
  cd "${srcdir}/glib-${pkgver}"

  ./configure --prefix=/usr --enable-static --enable-shared \
      --with-pcre=system --disable-fam || return 1
  make || return 1
  make DESTDIR="${pkgdir}" install || return 1

  install -d "${pkgdir}/etc/profile.d"
  install -m755 "${srcdir}/glib2.sh" "${pkgdir}/etc/profile.d/" || return 1
  install -m755 "${srcdir}/glib2.csh" "${pkgdir}/etc/profile.d/" || return 1

  chmod 755 ${pkgdir}/usr/bin/gtester-report || return 1
}
