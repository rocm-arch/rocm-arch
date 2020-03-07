# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=hip-nvcc
pkgver=3.1.0
pkgrel=1
pkgdesc="Heterogeneous Interface for Portability ROCm"
arch=('x86_64')
url="https://github.com/ROCm-Developer-Tools/HIP"
license=('custom')
makedepends=('libelf' 'git' 'cmake' 'cuda')
provides=('hip')
conflicts=('hip')
_dir="HIP-roc-${pkgver}"
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/ROCm-Developer-Tools/HIP/archive/roc-${pkgver}.tar.gz")
sha256sums=('3e7b7ef508f4c0d34756104936bc25eaffe419415013c93da72244aac199ae91')

build() {
  mkdir -p "${_dir}/build"
  cd "${_dir}/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/hip \
        ..
  make
}

package() {
  cd "${_dir}/build"

  make DESTDIR="$pkgdir" install

  install -d "$pkgdir/etc/ld.so.conf.d"
  cat << EOF > "$pkgdir/etc/ld.so.conf.d/hip.conf"
/opt/rocm/hip/lib
EOF
}
