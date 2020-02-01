# Maintainer: Markus Näther <naether.markus@gmail.com>
pkgname=rocblas
pkgver=3.0.0
pkgrel=1
pkgdesc="Next generation BLAS implementation for ROCm platform"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocBLAS"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja "hcc>=${pkgver}" python2 "rocm-comgr>=${pkgver}" rocminfo)
srcver="3.0"
source=("https://github.com/ROCmSoftwarePlatform/rocBLAS/archive/rocm-$srcver.tar.gz")
sha256sums=("c475bdfb78e90254eb5a1905c4fab4e382ae6a73cc93718b8bec563bce7b8edb")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # Tensile library needs python to be python2
  #export PATH="$srcdir:$PATH"
  #[[ -e "$srcdir/python" ]] || ln -s /usr/bin/python2 "$srcdir/python"

  # fix broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  # TODO: fix librocblas.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocblas" \
        -HIP_DIR="/opt/rocm/hip/lib/cmake/hip" \
        -hcc_DIR="/opt/rocm/hcc/lib/cmake/hcc" \
        -Damd_comgr_DIR="/opt/rocm/lib/cmake/amd_comgr" \
        -DBUILD_WITH_TENSILE=OFF \
        -G "Ninja" \
        "$srcdir/rocBLAS-rocm-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocblas.conf
    /opt/rocm/rocblas/lib/
		EOF
}
