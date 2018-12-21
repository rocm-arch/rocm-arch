# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=miopengemm
pkgver=1.1.5.9547fb98
pkgrel=1
pkgdesc="An OpenCL GEMM kernel generator"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/MIOpenGEMM"
license=('NCSAOSL')
depends=(ocl-icd)
makedepends=(git opencl-headers cmake gcc ninja ocl-icd)
source=("git+https://github.com/ROCmSoftwarePlatform/MIOpenGEMM.git#commit=9547fb9e8499a5a9f16da83b1e6b749de82dd9fb")
md5sums=('SKIP')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/miopen" \
        -G Ninja \
        "$srcdir/MIOpenGEMM"
  ninja
}

package() {
  ninja -C "$srcdir/build" install
}

