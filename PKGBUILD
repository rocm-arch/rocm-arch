# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hcc
pkgver=2.5.0
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
  "https://github.com/RadeonOpenCompute/llvm/archive/04d27535fcc4ca8250eaa86e01883c43901cd9ac.tar.gz"
  "https://github.com/RadeonOpenCompute/compiler-rt/archive/0713964c27a5b1766cf53bc83e549f523b2df98e.tar.gz"
  "https://github.com/RadeonOpenCompute/hcc-clang-upgrade/archive/7c03a2e9172656a0832bdb8e0a0c9cd2cbf634a1.tar.gz"
  "https://github.com/RadeonOpenCompute/clang-tools-extra/archive/90ebe0b8bd524c374e7ae71b44458ad1b0eb7307.tar.gz"
  "https://github.com/RadeonOpenCompute/lld/archive/7791abf961afe2591d67b6faee8204d9490631dc.tar.gz"
  "https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/6bfe121184303a6ccd731f45e7ba70bf23e4b3d8.tar.gz"
)
sha256sums=(
  "adbc9e358acf9c536c1ac9df87e303c8707d591491693768480a009952cb9413"
  "4cec6a27d078b5af5f0bb82dbf6a426a56fc94697520a288f45d583dc8ef4b5a"
  "ad51e4b0283aa955a5c829580921bf181f8ddee58a2a1398b5e942653c9c57a5"
  "f5896e0479bf8fcfd3dab825950d89625f9c8b4499c5820fd96aa1e47c368b53"
  "45fb7ec6c231f4c422a6a3a76ece5da2ef08d1d586ef1cfff6bee3c94aa32467"
  "58d1cec35dc539f5920856fe46de8daa2ca7ff636c6f06df1796a1e7b4c103b9"
  "0b933fe27ff1d401c78c3fc805c48268225286fe09ef6028d99967fe94ba382a"
)

prepare() {
  cd "$srcdir"
  mv -T llvm-04d27535fcc4ca8250eaa86e01883c43901cd9ac hcc-roc-$pkgver/compiler
  mv -T compiler-rt-0713964c27a5b1766cf53bc83e549f523b2df98e hcc-roc-$pkgver/compiler-rt
  mv -T hcc-clang-upgrade-7c03a2e9172656a0832bdb8e0a0c9cd2cbf634a1 hcc-roc-$pkgver/clang
  mv -T clang-tools-extra-90ebe0b8bd524c374e7ae71b44458ad1b0eb7307 hcc-roc-$pkgver/clang-tools-extra
  mv -T lld-7791abf961afe2591d67b6faee8204d9490631dc hcc-roc-$pkgver/lld
  mv -T ROCm-Device-Libs-6bfe121184303a6ccd731f45e7ba70bf23e4b3d8 hcc-roc-$pkgver/rocdl
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
