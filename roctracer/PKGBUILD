# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=roctracer
pkgver=3.1.0
pkgrel=1
pkgdesc="ROCm Tracer Callback/Activity Library for Performance tracing AMD GPU's"
arch=('x86_64')
url="https://github.com/ROCm-Developer-Tools/roctracer"
license=('MIT')
depends=('rocm' 'python' 'python-argparse' 'python-cppheaderparser')
makedepends=('cmake')
options=(!staticlibs strip)
source=("roctracer-roc-$pkgver.tar.gz::https://github.com/ROCm-Developer-Tools/roctracer/archive/roc-$pkgver.tar.gz")
sha256sums=('1339dac1d55f7bd2385894f9597fa0f1c8a50d9afe689951ea2dac78d22efdbb')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/roctracer \
        "$srcdir/roctracer-roc-$pkgver"
  make
}

package() {
  cd "$srcdir/build"

  make DESTDIR="$pkgdir" install

  # add links
  install -d "$pkgdir/usr/bin"
  local _fn
  for _fn in roctracer; do
    ln -s "/opt/rocm/roctracer/bin/$_fn" "$pkgdir/usr/bin/$_fn"
  done
}
