# Maintainer: Markus Näther <naether.markus@gmail.com>
pkgname=hipblas
pkgver=3.0.0
pkgrel=1
pkgdesc="Next generation BLAS implementation for ROCm platform"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/hipBLAS"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja "hcc>=${pkgver}" python2 "rocm-comgr>=${pkgver}" rocminfo)
srcver="3.0"
source=(
  "https://github.com/ROCmSoftwarePlatform/hipBLAS/archive/rocm-$srcver.tar.gz"
  "hipblas_hsa.patch"
 )
sha256sums=(
  "94dbe6d9b451ce80b407726353816415f5aee346547de5d3917aa5dfc5634323"
  'SKIP'
 )

prepare() {
	cd "${srcdir}/hipBLAS-rocm-$srcver"
	patch --forward --strip=1 --input="${srcdir}/hipblas_hsa.patch"
}

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
  
  #patch --forward --strip=1 --input="hipsparse_hsa.patch"

  # TODO: fix librocblas.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hipblas" \
        -Dhip_DIR="/opt/rocm/hip/lib/cmake/hip" \
        -Dhcc_DIR="/opt/rocm/hcc/lib/cmake/hcc" \
        -Damd_comgr_DIR="/opt/rocm/lib/cmake/amd_comgr" \
        -DBUILD_CLIENTS_SAMPLES=OFF \
        -DBUILD_CLIENTS_TESTS=OFF \
        -DHSA_PATH="/opt/rocm/hsa" \
        -G "Ninja" \
        "$srcdir/hipBLAS-rocm-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/hipblas.conf
    /opt/rocm/hipblas/lib/
		EOF
}
