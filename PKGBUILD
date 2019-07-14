# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hcc
pkgver=2.6.0
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
  "https://github.com/RadeonOpenCompute/hcc/archive/roc-hcc-$pkgver.tar.gz"
  "https://github.com/RadeonOpenCompute/llvm/archive/a42c925d178d2a3cd6541769279b736c56b3f935.tar.gz"
  "https://github.com/RadeonOpenCompute/compiler-rt/archive/28655ce202abf9edbbb309719b7b06be7a8f612d.tar.gz"
  "https://github.com/RadeonOpenCompute/hcc-clang-upgrade/archive/fff0bd8ccc310cbfec5e3e1bf516b81412081a71.tar.gz"
  "https://github.com/RadeonOpenCompute/clang-tools-extra/archive/0869af816d464dfcd9541c8ba0eca91c33411cc2.tar.gz"
  "https://github.com/RadeonOpenCompute/lld/archive/40c741790502f596c316fcf0216433630a8759de.tar.gz"
  "https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/6bfe121184303a6ccd731f45e7ba70bf23e4b3d8.tar.gz"
)
sha256sums=(
  "94abf5307bbe20b995f5aa74a1d047e44053af7feacbd0506218889bcf79f938"
  "fc6e2efc38eeb5d053b57f4f933cd66269db354b89ba7112f86a1430edac4bbe"
  "d3ade9067237abaf12e749330162ec57605d134f4670e83f3be2df6d22a3a148"
  "b45e25478b9c9208a040fb3b775f76835aaf88d4367192ab79aceed7b0459db3"
  "175dbb8d0a59886a93c19a79e34e75932c34d7c9233364035b9b9491483df267"
  "1987eeea137efe3f95e0e37e4c0df879681497803407143f23dbc71ad22203ea"
  "0b933fe27ff1d401c78c3fc805c48268225286fe09ef6028d99967fe94ba382a"
)

prepare() {
  cd "$srcdir"
  mv -T llvm-a42c925d178d2a3cd6541769279b736c56b3f935 hcc-roc-hcc-$pkgver/compiler
  mv -T compiler-rt-28655ce202abf9edbbb309719b7b06be7a8f612d hcc-roc-hcc-$pkgver/compiler-rt
  mv -T hcc-clang-upgrade-fff0bd8ccc310cbfec5e3e1bf516b81412081a71 hcc-roc-hcc-$pkgver/clang
  mv -T clang-tools-extra-0869af816d464dfcd9541c8ba0eca91c33411cc2 hcc-roc-hcc-$pkgver/clang-tools-extra
  mv -T lld-40c741790502f596c316fcf0216433630a8759de hcc-roc-hcc-$pkgver/lld
  mv -T ROCm-Device-Libs-6bfe121184303a6ccd731f45e7ba70bf23e4b3d8 hcc-roc-hcc-$pkgver/rocdl
}

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hcc" \
        -G Ninja \
        "$srcdir/hcc-roc-hcc-$pkgver"
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
