# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocalution
pkgver=3.0.0
pkgrel=1
pkgdesc="Next generation library for iterative sparse solvers for ROCm platform."
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocALUTION"
license=('NCSAOSL')
depends=(hcc hip rocsparse rocblas rocprim rocm-comgr)
makedepends=(git cmake gcc ninja "hcc>=${pkgver}" "hip>=${pkgver}" "rocsparse>=${pkgver}" "rocblas>=${pkgver}" "rocprim>=${pkgver}" "rocm-comgr>=${pkgver}" python2 rocminfo)
srcver="3.0"
source=("https://github.com/ROCmSoftwarePlatform/rocALUTION/archive/rocm-$srcver.tar.gz")
sha256sums=("b4666753aef8017c6c09da82880dd68cbff80ab41a967ef543ec344ee6848dc3")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # Tensile library needs python to be python2
  export PATH="$srcdir:$PATH"
  [[ -e "$srcdir/python" ]] || ln -s /usr/bin/python2 "$srcdir/python"

  # fix broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

#        -ROCBLAS_DIR="$pkgdir/opt/rocm/rocblas/lib/cmake/rocblas" \
  # TODO: fix librocalution.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocalution" \
        -ROCSPARSE_DIR="$pkgdir/opt/rocm/rocsparse/lib/cmake/rocsparse" \
        -Dhip_DIR="$pkgdir/opt/rocm/hip/lib/cmake/hip" \
        -Dhcc_DIR="$pkgdir/opt/rocm/hcc/lib/cmake/hcc" \
        -Damd_comgr_DIR="/opt/rocm/lib/cmake/amd_comgr" \
        -DBUILD_CLIENTS_SAMPLES=OFF \
        -DBUILD_CLIENTS_TESTS=OFF \
        -G Ninja \
        "$srcdir/rocALUTION-rocm-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocalution.conf
    /opt/rocm/rocalution/lib/
		EOF
}
