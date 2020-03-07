# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=hcc-git
pkgver=r7687.4fc4913a
pkgrel=1
pkgdesc="C++ Compiler for Heterogeneous Compute"
url="https://github.com/RadeonOpenCompute/hcc"
arch=(x86_64)
license=('Custom')
makedepends=(git cmake rocr-runtime)
provides=('hcc')
conflicts=('hcc')
_name=hcc
source=("git+https://github.com/RadeonOpenCompute/hcc.git"
        "git+https://github.com/RadeonOpenCompute/llvm-project.git"
        "git+https://github.com/RadeonOpenCompute/ROCm-Device-Libs.git")
sha256sums=("SKIP"
            "SKIP"
            "SKIP")

pkgver() {
  cd "$_name"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${srcdir}/${_name}"
  git submodule init
  git config submodule.llvm-project.git.url "${srcdir}/llvm-project"
  git config submodule.ROCm-Device-Libs.git.url "${srcdir}/rocdl"
  git submodule update
}

build() {

  cd "${srcdir}/${_name}"

  mkdir -p build

  cd "${srcdir}/${_name}/build"

  cmake .. -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX="${pkgdir}/opt/rocm/hcc"

  make
}

package() {
  cd "${srcdir}/${_name}/build"

  make DESTDIR=${pkgdir} install
}
