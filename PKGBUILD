# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=migraphx
pkgver=0.5.1
pkgrel=1
pkgdesc="AMD's graph optimization engine"
arch=('x86_64')
url="https://rocmsoftwareplatform.github.io/AMDMIGraphX/doc/html/"
license=('MIT')
depends=('rocm-cmake' 'miopen' 'hip' 'protobuf' 'half' 'pybind11')
makedepends=('cmake')
source=("migraphx-$pkgver.tar.gz::https://github.com/ROCmSoftwarePlatform/AMDMIGraphX/archive/$pkgver.tar.gz")
sha256sums=('2079dfe2e1efbe5e56015e056bf82f90d1645fa4a2610e20afa861697616e6af')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/migraphx \
        "$srcdir/AMDMIGraphX-$pkgver"
  make
}

package() {
  cd "$srcdir/build"

  make DESTDIR="$pkgdir" install

  # add links
  install -d "$pkgdir/usr/bin"
  local _fn
  for _fn in migraphx; do
    ln -s "/opt/rocm/migraphx/bin/$_fn" "$pkgdir/usr/bin/$_fn"
  done
}
