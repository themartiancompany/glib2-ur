# $Id: PKGBUILD,v 1.7 2008/04/01 17:05:21 jgc Exp $
# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.16.2
pkgrel=1
pkgdesc="Common C routines used by GTK+ 2.4 and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=7.6-3')
makedepends=('pkgconfig')
options=('!libtool')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.16/glib-${pkgver}.tar.bz2
	gkeyfile-bool-nocase.patch
	glib2.sh
	glib2.csh)
md5sums=('662224ad0186183f64de98ef2183454b'
         '5ca65611e824662146369e814d49ad06'
         '803017b365bd35dc20b092ce43b8c8c5'
         '90c7b830bef4baf225c2eb8b7ead0cab')

build() {
  cd ${startdir}/src/glib-${pkgver}
  patch -Np0 -i ${startdir}/src/gkeyfile-bool-nocase.patch || return 1
  ./configure --prefix=/usr --enable-static --enable-shared --with-pcre=system --disable-fam || return 1
  make || return 1
  make DESTDIR=${startdir}/pkg install || return 1

  install -d -m755 ${startdir}/pkg/etc/profile.d
  install -m755 ${startdir}/src/glib2.{csh,sh} ${startdir}/pkg/etc/profile.d/ || return 1
}
