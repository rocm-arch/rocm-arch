# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=aomp
pkgver=3.1.0
pkgrel=1
pkgdesc="Clang/LLVM based compiler with added support for the OpenMP API on
Radeon GPUs"
arch=('x86_64')
url="https://github.com/ROCm-Developer-Tools/aomp"
license=('Apache-2.0')
depends=('pciutils' 'numactl' 'libelf' 'libffi' 'python' 'openmpi')
makedepends=('cmake')
source=("aomp-$pkgver.tar.gz::https://github.com/ROCm-Developer-Tools/aomp/archive/roc-$pkgver.tar.gz")
sha256sums=('c27015dbe08546a70740a82ea6ed52b7f3d0819a82fc739ed03335e8cc9d67d2')

build() {
  cd ${srcdir}/aomp-roc-$pkgver
  nohup make
}

package() {
  cd ${srcdir}/aomp-roc-$pkgver
  make DESTDIR="$pkgdir" install
}
