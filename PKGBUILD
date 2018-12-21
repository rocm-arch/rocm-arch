# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocminfo
pkgver=1.0.0.1bb0cc7
pkgrel=1
pkgdesc="ROCm info tools - rocm_agent_enumerator"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/rocminfo"
license=('unknown')
makedepends=(git cmake ninja)
depends=(pciutils)
source=("git+https://github.com/RadeonOpenCompute/rocminfo.git#commit=1bb0ccc731f772bb1a553e37b41d06eb0a684926")
md5sums=("SKIP")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # TODO: fix rocminfo binary, it contains references to $srcdir

  cmake -DCMAKE_PREFIX_PATH=/opt/rocm \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" \
        -G Ninja \
        "$srcdir/rocminfo"
  ninja
}

package() {
  ninja -C "$srcdir/build" install
}

