# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocr-runtime
pkgver=2.0.0
pkgrel=2
pkgdesc="ROCm HSA"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/ROCR-Runtime"
license=('unknown')
makedepends=(git cmake gcc ninja)
depends=('roct-thunk-interface')
source=("git+https://github.com/RadeonOpenCompute/ROCR-Runtime.git#tag=roc-2.0.0")
md5sums=("SKIP")

build() {
  mkdir -p $srcdir/build
  cd $srcdir/build
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=$pkgdir/opt/rocm \
        -DHSAKMT_INC_PATH=/opt/rocm/include \
        -DHSAKMT_LIB_PATH=/opt/rocm/lib64 \
        -G Ninja \
        $srcdir/ROCR-Runtime/src
  ninja
}

package() {
  ninja -C $srcdir/build install
  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocm-runtime.conf
		/opt/rocm/lib
    /opt/rocm/hsa/lib/
		EOF

  # Link a .so.1, because the version bump broke the library names
  ln -s /opt/rocm/hsa/lib/libhsa-runtime64.so.2.0.0 "$pkgdir/opt/rocm/hsa/lib/libhsa-runtime64.so.1"
}
