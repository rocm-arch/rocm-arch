# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=roctracer
pkgver=3.3.0
pkgrel=3
pkgdesc="ROCm Tracer Callback/Activity Library for Performance tracing AMD GPU's"
arch=('x86_64')
url="https://github.com/ROCm-Developer-Tools/roctracer"
license=('MIT')
depends=('hsa-rocr' 'python' 'python-argparse' 'python-cppheaderparser')
optdepends=('hip: tracing for higher level interface')
makedepends=('cmake')
options=(!staticlibs strip)
source=("roctracer-roc-$pkgver.tar.gz::https://github.com/ROCm-Developer-Tools/roctracer/archive/roc-$pkgver.tar.gz")
sha256sums=('4a3b12e9d2a78565f4c6eaaa23c696c9fd8d57cb75d5e57e834def1624e68770')

prepare() {
    local _fn
    for _fn in $srcdir/roctracer-roc-$pkgver/{bin,script}/*.py; do
        2to3 -w $_fn
    done
}

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm \
        "$srcdir/roctracer-roc-$pkgver"
  make
}

package() {
  cd "$srcdir/build"

  make DESTDIR="$pkgdir" install
}
