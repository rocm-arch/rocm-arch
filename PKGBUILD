# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocr-runtime
pkgver=2.6.0
pkgrel=1
pkgdesc="ROCm HSA"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/ROCR-Runtime"
license=('unknown')
makedepends=(git cmake gcc ninja)
depends=('roct-thunk-interface')
source=("https://github.com/RadeonOpenCompute/ROCR-Runtime/archive/roc-$pkgver.tar.gz")
sha256sums=("46636716ef9f222e90cd2f3d54acf9bf50c4088cd55178230628bd195833de88")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=$pkgdir/opt/rocm \
        -DCMAKE_PREFIX_PATH=/opt/rocm/libhsakmt \
        -DHSAKMT_INC_PATH=/opt/rocm/include \
        -DHSAKMT_LIB_PATH=/opt/rocm/lib64 \
        -G Ninja \
        "$srcdir/ROCR-Runtime-roc-$pkgver/src"
  ninja
}

package() {
  ninja -C "$srcdir/build" install
  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocm-runtime.conf
		/opt/rocm/lib
    /opt/rocm/hsa/lib/
		EOF
}
