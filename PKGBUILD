# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=roct-thunk-interface-git
pkgver=r646.ea01eb3
pkgrel=1
pkgdesc="Radeon Open Compute Thunk Interface"
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
arch=(x86_64)
license=('MIT')
makedepends=(git cmake gcc)
depends=(numactl pciutils)
provides=('roct-thunk-interface')
conflicts=('roct-thunk-interface')
_name=ROCT-Thunk-Interface
source=("git+https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface.git")
sha256sums=('SKIP')

pkgver() {
  cd "$_name"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  mkdir -p "$srcdir/${_name}/build"
  cd "$srcdir/${_name}/build"
  cmake .. -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="${pkgdir}/usr" \

  make
}

package() {
  cd "${srcdir}/${_name}/build"
  make DESDIR=${pkgdir} install
}
