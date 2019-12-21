# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=hcc-git
pkgver=r7685.e8488139
pkgrel=1
pkgdesc="C++ Compiler for Heterogeneous Compute"
url="https://github.com/RadeonOpenCompute/hcc"
arch=(x86_64)
license=('Custom')
makedepends=(git cmake rocr-runtime)
provides=('hcc')
conflicts=('hcc')
_name=hcc
source=("git+https://github.com/RadeonOpenCompute/hcc.git")
sha256sums=("SKIP")

pkgver() {
  cd "$_name"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {

  cd "${srcdir}/${_name}"

  mkdir -p build

  cd "${srcdir}/${_name}/build"

  cmake .. -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_LIBDIR=lib \
    -DCMAKE_INSTALL_PREFIX=/usr

  make
}

package() {
  cd "${srcdir}/${_name}/build"

  make DESTDIR=${pkgdir} install
}
