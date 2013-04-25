# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=glib2
pkgname=(glib2 glib2-docs)
pkgver=2.36.1
pkgrel=3
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
makedepends=('pkg-config' 'python2' 'libxslt' 'docbook-xml' 'pcre' 'libffi' 'elfutils')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/${pkgver%.*}/glib-$pkgver.tar.xz
        revert-warn-glib-compile-schemas.patch
        gvariant-fix-annotation.patch
        partially-revert-ce0022933c255313e010b27f977f4ae02aad1e7e.patch)
sha256sums=('7de37586794e92c024feebe5d306bf5f245fef4803c3666af1ae8dac6ee10b24'
            '049240975cd2f1c88fbe7deb28af14d4ec7d2640495f7ca8980d873bb710cc97'
            'ebbb0581322b1fc546f93f9d77f39f37584004086d2f6f2637a8bb7894e36b2b'
            '5928ac4fd114cda846fe38a3b8bedc5b038dbf9e47f76029af7d75e5dc8ae5be')

build() {
  cd glib-$pkgver

  # fix FS#34630 https://bugs.archlinux.org/task/34630
  export CFLAGS+=" -Wall"

  # Upstream fixes from 2.36 branch
  patch -Np1 -i ../gvariant-fix-annotation.patch
  patch -Np1 -i ../partially-revert-ce0022933c255313e010b27f977f4ae02aad1e7e.patch

  patch -Rp1 -i ../revert-warn-glib-compile-schemas.patch
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --libdir=/usr/lib \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

check() {
  cd glib-$pkgver
  #make -k check || :
}

package_glib2() {
  depends=('pcre' 'libffi')
  optdepends=('python2: for gdbus-codegen and gtester-report'
              'elfutils: gresource inspection tool')
  options=('!docs' '!libtool' '!emptydirs')
  license=('LGPL')

  cd glib-$pkgver
  make completiondir=/usr/share/bash-completion/completions DESTDIR="$pkgdir" install

  for _i in "$pkgdir/usr/share/bash-completion/completions/"*; do
      chmod -x "$_i"
  done

  # Our gdb does not ship the required python modules, so remove it
  rm -rf "$pkgdir/usr/share/gdb/"
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

  rm -rf "${pkgdir}/usr/share/man"
}
