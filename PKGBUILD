# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=rocm-smi-lib
pkgver=3.1.0
pkgrel=1
pkgdesc="ROCm SMI LIB"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/rocm_smi_lib"
license=('custom:NCSAOSL')
depends=()
makedepends=('cmake')
options=(!staticlibs strip)
source=("rocm_smi_lib-roc-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/rocm_smi_lib/archive/roc-$pkgver.tar.gz")
sha256sums=('a4f61b1b8792e64489e0c45366d84736164be2405ce8ad685cee9fa49c18e9a3')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/rocm_smi_lib \
        "$srcdir/rocm_smi_lib-roc-$pkgver"
  make
}

package() {
  cd "$srcdir/build"

  make DESTDIR="$pkgdir" install
}
