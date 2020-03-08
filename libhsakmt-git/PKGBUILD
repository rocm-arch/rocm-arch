# Maintainer:  Aqeel Akber <self@aqeelakber.com>
pkgname=libhsakmt-git
pkgver=1.0.0.2.gfd24c14
pkgrel=1
pkgdesc="AMD HSA KMT Library (freedesktop)"
arch=('x86_64')
url="https://cgit.freedesktop.org/amd/hsakmt/"
license=('custom:AMD')
provides=("libhsakmt")
source=(libhsakmt::git://anongit.freedesktop.org/amd/hsakmt)
md5sums=('SKIP')

pkgver(){
  cd "$srcdir/libhsakmt"
  git describe --long --tags | sed 's/hsakmt-//;s/-/./;s/-/./'
}


build() {
  cd "$srcdir/libhsakmt"
  ./autogen.sh
  make
}

check() {
  cd "$srcdir/libhsakmt"
  make -k check
}

package() {
  cd "$srcdir/libhsakmt"
  make DESTDIR="$pkgdir/" install
}

# vim:set ts=2 sw=2 et:
