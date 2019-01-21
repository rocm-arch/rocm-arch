# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=roct-thunk-interface
pkgver=2.0.0
pkgrel=2
pkgdesc="ROCm HSA"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
license=('unknown')
makedepends=(git cmake gcc ninja) 
depends=(numactl)
source=("git+https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface.git#tag=roc-2.0.0")
sha256sums=("SKIP")

build() {
  mkdir -p $srcdir/build
  cd $srcdir/build
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=$pkgdir/opt/rocm \
        -G Ninja \
        $srcdir/ROCT-Thunk-Interface
  ninja all build-dev
}

package() {
  ninja -C $srcdir/build install install-dev
  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/roct-tunk-interface.conf
		/opt/rocm/lib64
		EOF
}

