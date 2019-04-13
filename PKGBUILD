# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hcc
pkgver=2.3.0
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
  "https://github.com/RadeonOpenCompute/llvm/archive/87f982f8ce2b85ce824f91bf8c2c90f6843a50a3.tar.gz"
  "https://github.com/RadeonOpenCompute/compiler-rt/archive/bcfc4b7850d4031918c900877d6d3428e07a06e9.tar.gz"
  "https://github.com/RadeonOpenCompute/hcc-clang-upgrade/archive/785f31db116e742ac53d052e207979869a857d1a.tar.gz"
  "https://github.com/RadeonOpenCompute/clang-tools-extra/archive/2e8ff7e2d07754b9d943f7b57b3bfcd5a5102df6.tar.gz"
  "https://github.com/RadeonOpenCompute/lld/archive/f0cfa52cd79dce6d4d5fcc5105e676c601dc2d3d.tar.gz"
  "https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/ae81146538cea205c1463c0035f13cd67d3ee44d.tar.gz"
)
sha256sums=(
  "0cdb3a2646db3dd2219bb313d81453b0c16d8152b12c6ed133212480b1962ee2"
  "28c2ec9fcb2c0d25cbf863fda510666f1aaf324653ac051f3d5ed2897ebc44cc"
  "b4cabc86ecfef081b6f96ce30ca7288abf97de419cc6524ab1c569562b2b4ac8"
  "ee5a44efbe90cdc431f2e60121083c71c6765a290d434a78b471a2a52171a105"
  "19a7e31657e6db3a5b54f230b09730396d05efa354e376e15363cd5deb874e0b"
  "321aecad28e890831f37817cd395f08a4dd5c0c19d306cd150219fdc369ce885"
  "4270ceaee17f7c901170d4d3115f53c1ec3554e517b376f7fee11d2e63f66e70"
)

prepare() {
  cd "$srcdir"
  mv -T llvm-87f982f8ce2b85ce824f91bf8c2c90f6843a50a3 hcc-roc-$pkgver/compiler
  mv -T compiler-rt-bcfc4b7850d4031918c900877d6d3428e07a06e9 hcc-roc-$pkgver/compiler-rt
  mv -T hcc-clang-upgrade-785f31db116e742ac53d052e207979869a857d1a hcc-roc-$pkgver/clang
  mv -T clang-tools-extra-2e8ff7e2d07754b9d943f7b57b3bfcd5a5102df6 hcc-roc-$pkgver/clang-tools-extra
  mv -T lld-f0cfa52cd79dce6d4d5fcc5105e676c601dc2d3d hcc-roc-$pkgver/lld
  mv -T ROCm-Device-Libs-ae81146538cea205c1463c0035f13cd67d3ee44d hcc-roc-$pkgver/rocdl
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
