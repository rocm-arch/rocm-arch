# Maintainer: acxz <akashpatel2008 at yahoo dot com>
# Contributor: Jakub Okoński <jakub@okonski.org>
pkgname=roct-thunk-interface
pkgver=3.0.0
pkgrel=1
pkgdesc="Radeon Open Compute Thunk Interface"
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
arch=(x86_64)
license=('MIT')
makedepends=(cmake gcc)
depends=(numactl pciutils)
_name=ROCT-Thunk-Interface-roc
source=("https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface/archive/roc-$pkgver.tar.gz")
sha256sums=("d6d91397fcd926bd90fae8dfe81d69653273e8b68cd47f822576838f2ac96729")

build() {
  mkdir -p "$srcdir/${_name}-${pkgver}/build"
  cd "$srcdir/${_name}-${pkgver}/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" \
        ..

  make all build-dev
}

package() {
  cd "${srcdir}/${_name}-${pkgver}/build"
  make DESDIR=${pkgdir} install install-dev

  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/roct-thunk-interface.conf
		/opt/rocm/lib
		EOF
}
