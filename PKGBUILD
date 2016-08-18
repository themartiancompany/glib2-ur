# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.48.2
pkgrel=1
pkgdesc="Low level core library"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=('pkg-config' 'python' 'libxslt' 'docbook-xml' 'pcre' 'libffi' 'libelf')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver:0:4}/glib-$pkgver.tar.xz
        glib-compile-schemas.hook
        gio-querymodules.hook
        revert-warn-glib-compile-schemas.patch)
sha256sums=('f25e751589cb1a58826eac24fbd4186cda4518af772806b666a3f91f66e6d3f4'
            'e1123a5d85d2445faac33f6dae1085fdd620d83279a4e130a83fe38db52b62b3'
            '5ba204a2686304b1454d401a39a9d27d09dd25e4529664e3fd565be3d439f8b6'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97')

prepare() {
  cd glib-$pkgver
  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
}
  
build() {
  cd glib-$pkgver
  ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

check() {
  cd glib-$pkgver
  # Takes an effing long time
  #make -k check || :
}

package_glib2() {
  depends=('pcre' 'libffi')
  optdepends=('python: for gdbus-codegen and gtester-report'
              'libelf: gresource inspection tool')
  options=('!docs' '!emptydirs')
  license=('LGPL')

  cd glib-$pkgver
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  chmod -x "$pkgdir"/usr/share/bash-completion/completions/*

  # Our gdb does not ship the required python modules, so remove it
  rm -r "$pkgdir/usr/share/gdb/"
  
  # install hooks
  install -dm755 "$pkgdir"/usr/share/libalpm/hooks/
  install -m644 "$srcdir"/{glib-compile-schemas,gio-querymodules}.hook "$pkgdir"/usr/share/libalpm/hooks/
}

package_glib2-docs() {
  pkgdesc="Documentation for glib2"
  conflicts=('gobject2-docs')
  replaces=('gobject2-docs')
  license=('custom')
  options=('docs' '!emptydirs')
  
  cd glib-$pkgver/docs
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/usr/share/licenses/glib2-docs"
  install -m644 reference/COPYING "${pkgdir}/usr/share/licenses/glib2-docs/"
}
