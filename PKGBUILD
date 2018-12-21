# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=miopen
pkgver=1.7.0
pkgrel=2
pkgdesc="AMD's Machine Intelligence Library"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/MIOpen"
license=('NCSAOSL')
depends=(ocl-icd hip hcc rocblas "boost>=1.58")
makedepends=(git opencl-headers cmake gcc ninja ocl-icd hip hcc rocblas "boost>=1.58" miopengemm)
source=("git+https://github.com/ROCmSoftwarePlatform/MIOpen.git#tag=1.7.0")
md5sums=('SKIP')

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
        "$srcdir/MIOpen"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/miopen.conf
    /opt/rocm/miopen/lib/
		EOF
}

