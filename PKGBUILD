# Maintainer: acxz <akashpatel2008 at yahoo dot com>

pkgname=hipcpu-git
pkgver=r18.d5b28e3
pkgrel=1
pkgdesc="Implementation of AMD HIP for CPUs"
arch=('i686' 'x86_64')
url="http://github.com/illuhad/hipCPU"
license=('BSD 2-Clause')
depends=('openmp')
makedepends=('git' 'cmake')
provides=('hipcpu')
conflicts=('hipcpu')
_name=hipCPU
source=("git+https://github.com/illuhad/hipCPU.git")
sha256sums=('SKIP')

pkgver() {
  cd "$_name"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {

  cd "${srcdir}/${_name}"

  mkdir -p build

  cd "${srcdir}/${_name}/build"

  cmake .. -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr

  make
}

package() {
  cd "${srcdir}/${_name}/build"

  make DESTDIR=${pkgdir} install
}
