# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=atmi
pkgver=3.1.0
pkgrel=1
pkgdesc="Asynchronous Task and Memory Interface (ATMI) is a task graph framework for heterogeneous CPU-GPU systems."
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/atmi"
license=('MIT')
depends=('rocm-cmake' 'rocm-device-libs')
makedepends=('cmake')
source=("atmi-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/atmi/archive/rocm_$pkgver.tar.gz")
sha256sums=('f2b072d79bb6987b638edabcf5aec7250079bf9aa60ca0c50b659744b8ed5f80')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/atmi \
        "$srcdir/atmi-rocm_$pkgver/src"
  make
}

package() {
  cd "$srcdir/build"

  make DESTDIR="$pkgdir" install
}
