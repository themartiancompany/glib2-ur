# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=glib2
pkgver=2.35.9
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre' 'libffi')
makedepends=('pkg-config' 'python' 'libxslt' 'docbook-xml')
optdepends=('python: for gdbus-codegen and gtester-report')
options=('!libtool' '!docs' '!emptydirs')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver%.*}/glib-$pkgver.tar.xz
        revert-warn-glib-compile-schemas.patch
        0001-Make-gtester-report-work-with-Python-3.x.patch)
sha256sums=('75ce10a4c47c830234ce6af8e52dc4ebaf603a9c04b4114eb22dd00335f3943e'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97'
            'a2b4ff3836fc0a5b03b8e6bfa69be44b9a65f16486e4c1e4bc6c3d50fe2ad632')

build() {
  cd glib-$pkgver
  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
  patch -Np1 -i ../0001-Make-gtester-report-work-with-Python-3.x.patch
  ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

package() {
  cd glib-$pkgver
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  for _i in "$pkgdir/usr/share/bash-completion/completions/"*; do
      chmod -x "$_i"
  done
}
