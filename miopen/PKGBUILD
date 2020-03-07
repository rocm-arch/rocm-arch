# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=miopen
# upstream has different versioning, let's keep it at 2.3.0 until they catch up
_srcver=1.8.1
pkgver=2.3.0.$_srcver
pkgrel=2
pkgdesc="AMD's Machine Intelligence Library"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/MIOpen"
license=('NCSAOSL')
depends=(ocl-icd hip hcc rocblas "boost>=1.58" clang-ocl)
makedepends=(git opencl-headers cmake gcc ninja ocl-icd hip hcc rocblas "boost>=1.58" miopengemm)
source=("https://github.com/ROCmSoftwarePlatform/MIOpen/archive/$_srcver.tar.gz")
sha256sums=('58941138cea6432d0a9218c1c9af1784587bff58501b8a38d7f634fc544321a3')

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
        "$srcdir/MIOpen-$_srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/miopen.conf
    /opt/rocm/miopen/lib/
		EOF
}

