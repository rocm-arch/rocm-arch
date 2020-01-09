# Maintainer: Jakub Okoński <jakub@okonski.org>
# Maintainer: Markus Näther <naetherm@cs.uni-freiburg.de>
pkgname=rocminfo
pkgver=3.0.0
pkgrel=1
pkgdesc="ROCm info tools - rocm_agent_enumerator"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/rocminfo"
license=('unknown')
makedepends=(git cmake ninja)
depends=(pciutils rocm-cmake rocr-runtime roct-thunk-interface)
source=("git+https://github.com/RadeonOpenCompute/rocminfo.git#tag=roc-$pkgver")
md5sums=("SKIP")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_PREFIX_PATH=/opt/rocm \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" \
        -DROCM_DIR=/opt/rocm \
        -G Ninja \
        "$srcdir/rocminfo"
  ninja
}

package() {
  ninja -C "$srcdir/build" install
}
