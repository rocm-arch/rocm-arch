# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
# Maintainer: Rigo Reddig <rigo.reddig@gmail.com> ;)
# Maintainer: Markus Näther <naetherm@cs.uni-freiburg.de>
pkgname=hcc
pkgver=3.0.0
pkgrel=1
pkgdesc="HCC is an Open Source, Optimizing C++ Compiler for Heterogeneous Compute"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/hcc"
license=('NCSAOSL')
depends=('rocr-runtime>=3.0.0' z3)
makedepends=(git cmake gcc ninja z3)
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
options=(!staticlibs strip)
_srcver=3.0.0
source=(
  "hcc.tar.gz::https://github.com/RadeonOpenCompute/hcc/archive/roc-hcc-$_srcver.tar.gz"
  "llvm.tar.gz::https://github.com/RadeonOpenCompute/llvm-project/archive/roc-hcc-$_srcver.tar.gz"
  "ROCm-Device-Libs.tar.gz::https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/roc-hcc-$_srcver.tar.gz"
)
sha256sums=('0eb296710278a2abb72e6875a45fed7c2772e744827f3e04a2ef34d8978a03b7'
            '5b91197eeb724e1c6a042658aa314b60073ec6f6af9a0551409164d9d15287cd'
            'ba8d40ed8b39f3285e00a27716905edff3792929168735bd4b19eb4f1f15aff8')

prepare() {
  cd "$srcdir"
  mv -T llvm-project-roc-hcc-$_srcver hcc-roc-hcc-$_srcver/llvm-project
  mv -T ROCm-Device-Libs-roc-hcc-$_srcver hcc-roc-hcc-$_srcver/rocdl
}

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hcc" \
        -DLLVM_INSTALL_UTILS=TRUE \
        -G Ninja \
        "$srcdir/hcc-roc-hcc-$_srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  # add links
  mkdir -p "$pkgdir/usr/bin"
  for fn in hcc hcc-config
  do
    ln -s /opt/rocm/hcc/bin/$fn "$pkgdir/usr/bin/$fn"
  done

  # additional link to make hcc demos happy
  mkdir -p "$pkgdir/opt/rocm/include"
  ln -s /opt/rocm/hcc/include "$pkgdir/opt/rocm/include/hcc"

  # additional link to make clang-ocl work
  ln -s /opt/rocm/hcc/bin "$pkgdir/opt/rocm/hcc/bin/x86_64"
}
