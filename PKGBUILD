# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.18.1
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
md5sums=('51a9a33f49a4896d4d95d8e980666b9e'
         '803017b365bd35dc20b092ce43b8c8c5'
         '90c7b830bef4baf225c2eb8b7ead0cab')

build() {
  cd ${startdir}/src/glib-${pkgver}
  ./configure --prefix=/usr --enable-static --enable-shared --with-pcre=system --disable-fam || return 1
  make || return 1
  make DESTDIR=${startdir}/pkg install || return 1

  install -d -m755 ${startdir}/pkg/etc/profile.d
  install -m755 ${startdir}/src/glib2.{csh,sh} ${startdir}/pkg/etc/profile.d/ || return 1
}
