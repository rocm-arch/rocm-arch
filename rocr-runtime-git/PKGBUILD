# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=rocr-runtime-git
pkgver=r94.cd47351
pkgrel=1
pkgdesc="ROCm Platform Runtime: ROCr a HPC market enhanced HSA based runtime"
url="https://github.com/RadeonOpenCompute/ROCR-Runtime"
arch=(x86_64)
license=('NCSA')
makedepends=(git cmake gcc libelf roct-thunk-interface)
depends=()
provides=('rocr-runtime')
conflicts=('rocr-runtime')
_name=ROCR-Runtime
source=("git+https://github.com/RadeonOpenCompute/ROCR-Runtime.git")
sha256sums=("SKIP")

pkgver() {
  cd "$_name"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  mkdir -p "$srcdir/${_name}/src/build"
  cd "$srcdir/${_name}/src/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="${pkgdir}/opt/rocm" \
        -DHSAKMT_INC_PATH=/opt/rocm/include \
        -DHSAKMT_LIB_PATH=/opt/rocm/lib \
        ..
  make
}

package() {
  cd "${srcdir}/${_name}/src/build"
  make DESDIR=${pkgdir} install
  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocm-runtime.conf
    /opt/rocm/lib
    /opt/rocm/hsa/lib/
	EOF
}
