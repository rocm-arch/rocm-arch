# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=miopen
pkgver=2.3.0
pkgrel=1
pkgdesc="AMD's Machine Intelligence Library"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/MIOpen"
license=('NCSAOSL')
depends=(ocl-icd hip hcc rocblas "boost>=1.58" clang-ocl)
makedepends=(git opencl-headers cmake gcc ninja ocl-icd hip hcc rocblas "boost>=1.58" miopengemm)
source=("https://github.com/ROCmSoftwarePlatform/MIOpen/archive/roc-$pkgver.tar.gz")
sha256sums=('eba4dc29743ce35a3d3ce614c732c12e04356c6e0b1e33f5880e15f06b535ceb')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  # TODO: fix libMIOpen.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/miopen" \
        -DMIOPEN_BACKEND=HIP \
        -DCMAKE_PREFIX_PATH="/opt/rocm/hcc;/opt/rocm/hip" \
        -DHALF_INCLUDE_DIR="$startdir" \
        -G Ninja \
        "$srcdir/MIOpen-roc-$pkgver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/miopen.conf
    /opt/rocm/miopen/lib/
		EOF
}

