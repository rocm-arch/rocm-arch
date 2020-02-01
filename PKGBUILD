# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=hipsparse
pkgver=3.0.0
pkgrel=1
pkgdesc="ROCm SPARSE marshalling library."
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/hipSPARSE"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja "hcc>=${pkgver}" "hip>=${pkgver}" python2 "rocprim>=${pkgver}" "rocsparse>=${pkgver}" "rocm-comgr>=${pkgver}" rocminfo)
srcver="3.0"
source=(
  "https://github.com/ROCmSoftwarePlatform/hipSPARSE/archive/rocm-$srcver.tar.gz"
  "hipsparse_hsa.patch"
)
sha256sums=(
  "389de1d151436acdaa89c33530e7b86f6c4dc4aecbf3466131ec0972d8d1c33e"
  'SKIP'
)

prepare() {
	cd "${srcdir}/hipSPARSE-rocm-$srcver"
	patch --forward --strip=1 --input="${srcdir}/hipsparse_hsa.patch"
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
  
  # TODO: fix librocsparse.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hipsparse" \
        -Dhcc_DIR="/opt/rocm/hcc/lib/cmake/hcc" \
        -Dhip_DIR="/opt/rocm/hip/lib/cmake/hip" \
        -Damd_comgr_DIR="/opt/rocm/lib/cmake/amd_comgr" \
        -Drocsparse_DIR="/opt/rocm/rocsparse/lib/cmake/rocsparse" \
        -DBUILD_CLIENTS_SAMPLES=OFF \
        -DBUILD_CLIENTS_TESTS=OFF \
        -DHSA_HEADER="/opt/rocm/hsa/include" \
        -G Ninja \
        "$srcdir/hipSPARSE-rocm-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/hipsparse.conf
    /opt/rocm/hipsparse/lib/
		EOF
}
