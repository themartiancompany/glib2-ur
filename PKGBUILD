# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.24.0
pkgrel=1
pkgdesc="Common C routines used by GTK+ 2.4 and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=8.02')
makedepends=('pkgconfig')
options=('!libtool' '!docs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.24/glib-${pkgver}.tar.bz2
        glib2.sh
        glib2.csh)
sha256sums=('7b6aa2cf21e734a6092a711bf196b8d2ddc589b971f93337610c10fa4f23400d'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3')

build() {
  cd "${srcdir}/glib-${pkgver}"
  ./configure --prefix=/usr --enable-static --enable-shared \
      --with-pcre=system --disable-fam || return 1
  make || return 1
  make DESTDIR="${pkgdir}" install || return 1

  install -d "${pkgdir}/etc/profile.d"
  install -m755 "${srcdir}/glib2.sh" "${pkgdir}/etc/profile.d/" || return 1
  install -m755 "${srcdir}/glib2.csh" "${pkgdir}/etc/profile.d/" || return 1
}
