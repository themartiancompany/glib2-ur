# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.18.3
pkgrel=1
pkgdesc="Common C routines used by GTK+ 2.4 and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=7.8')
makedepends=('pkgconfig')
options=('!libtool' '!docs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.18/glib-${pkgver}.tar.bz2
	glib2.sh
	glib2.csh)
md5sums=('f13996a7bd57525d796a6593f26a7771'
         '803017b365bd35dc20b092ce43b8c8c5'
         '90c7b830bef4baf225c2eb8b7ead0cab')

build() {
  cd "${srcdir}/glib-${pkgver}"
  ./configure --prefix=/usr --enable-static --enable-shared --with-pcre=system --disable-fam || return 1
  make || return 1
  make DESTDIR="${pkgdir}" install || return 1

  install -d -m755 "${pkgdir}/etc/profile.d"
  install -m755 "${srcdir}/glib2.sh" "${pkgdir}/etc/profile.d/" || return 1
  install -m755 "${srcdir}/glib2.csh" "${pkgdir}/etc/profile.d/" || return 1
}
