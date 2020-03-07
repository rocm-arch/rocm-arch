# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=miopengemm
pkgver=1.1.5.5c980d5
pkgrel=4
pkgdesc="An OpenCL GEMM kernel generator"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/MIOpenGEMM"
license=('NCSAOSL')
depends=(ocl-icd)
makedepends=(git opencl-headers cmake gcc ninja ocl-icd rocm-cmake-git)
srcver="9547fb9e8499a5a9f16da83b1e6b749de82dd9fb"
source=("https://github.com/ROCmSoftwarePlatform/MIOpenGEMM/archive/$srcver.tar.gz")
sha256sums=('7f56bfb9344c5335166900d38f3ddce90a275c34efeb7c65e1346d536ef30f4a')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" \
        -G Ninja \
        "$srcdir/MIOpenGEMM-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install
}

