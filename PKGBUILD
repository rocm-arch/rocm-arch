# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hcc
pkgver=2.4.0
pkgrel=1
pkgdesc="HCC is an Open Source, Optimizing C++ Compiler for Heterogeneous Compute"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/hcc"
license=('NCSAOSL')
depends=(rocr-runtime z3)
makedepends=(git cmake gcc ninja z3)
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
options=(!staticlibs strip)
source=(
  "https://github.com/RadeonOpenCompute/hcc/archive/roc-$pkgver.tar.gz"
  "https://github.com/RadeonOpenCompute/llvm/archive/c17eb49ccf5d95f2a1519755b291f2ab104780e2.tar.gz"
  "https://github.com/RadeonOpenCompute/compiler-rt/archive/9e3381899d79c1b3aac95aca4338e2c3ed9331ba.tar.gz"
  "https://github.com/RadeonOpenCompute/hcc-clang-upgrade/archive/7edf06e2e0e1aef72fdb3c5f8f52faa799cad805.tar.gz"
  "https://github.com/RadeonOpenCompute/clang-tools-extra/archive/4e4e9806c0396857e47e8269001171ee84ad174e.tar.gz"
  "https://github.com/RadeonOpenCompute/lld/archive/25e9d58ee6ac676da6c7b50682cd4268c9abcb74.tar.gz"
  "https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/8457aa2f37c8d0e5e232ef6456fb09ef8a50b466.tar.gz"
)
sha256sums=(
  "619c5e912684a12ea29e2dccb335d666ba48ee7f9f8816e5feccc69b0474160e"
  "64f22d50363e93d45c6641e14ce58cf86cbe0ba60c9df6d30b7e3b714f77e70c"
  "f61ec5848c0f895621247f73f4d0058be4477b431781546e0b3242339bc4f27f"
  "c630ae7e517008f96de16412807bd26713002bfe131029989f3fef892d0a791b"
  "7bcaa7df321ca84f44027b466fd1151bef20eb6fddcef91da4eecc7823f0ca8d"
  "f227938edea1ce05e219629ad19b3adfe89e98ffac88b2bbb947ca01360c56d6"
  "912493c7d1e53a87fac3d30a35bbc1c40dd6f0981079116749b9279586c02a3f"
)

prepare() {
  cd "$srcdir"
  mv -T llvm-c17eb49ccf5d95f2a1519755b291f2ab104780e2 hcc-roc-$pkgver/compiler
  mv -T compiler-rt-9e3381899d79c1b3aac95aca4338e2c3ed9331ba hcc-roc-$pkgver/compiler-rt
  mv -T hcc-clang-upgrade-7edf06e2e0e1aef72fdb3c5f8f52faa799cad805 hcc-roc-$pkgver/clang
  mv -T clang-tools-extra-4e4e9806c0396857e47e8269001171ee84ad174e hcc-roc-$pkgver/clang-tools-extra
  mv -T lld-25e9d58ee6ac676da6c7b50682cd4268c9abcb74 hcc-roc-$pkgver/lld
  mv -T ROCm-Device-Libs-8457aa2f37c8d0e5e232ef6456fb09ef8a50b466 hcc-roc-$pkgver/rocdl
}

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hcc" \
        -G Ninja \
        "$srcdir/hcc-roc-$pkgver"
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
