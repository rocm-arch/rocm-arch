# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hcc
pkgver=2.2.0
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
  "https://github.com/RadeonOpenCompute/llvm/archive/68584f0b7bc07d43af64f90b3726988b5a513bf9.tar.gz"
  "https://github.com/RadeonOpenCompute/compiler-rt/archive/15e4a1f9195d3d90828a7a122d866c69e650155c.tar.gz"
  "https://github.com/RadeonOpenCompute/hcc-clang-upgrade/archive/c792478f19beee13540053f188094898a008d245.tar.gz"
  "https://github.com/RadeonOpenCompute/clang-tools-extra/archive/b4a0f4adacc1dd5b2deecda2282b20af963716e7.tar.gz"
  "https://github.com/RadeonOpenCompute/lld/archive/8e7027a1bf3b2a0007562d2164e0fa4c037a31d5.tar.gz"
  "https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/381a895a7e80d6ff0785faa21396e66c54d81f85.tar.gz"
)
sha256sums=(
  "8a1b6d57fe14dcbfdee11aa9cee7d6364f61058cb6f174583f78c68611704864"
  "4667582ff6f49b6e864dd830bf949fe3e7f20873cfa484dab31605fb643afcf8"
  "3c653797405cbbefbb592435a340ad31f8c21467849c49992319ed007081aa7e"
  "fc3a7f7feb6d53cf292be0d6850a71055eaf68a5d28fdcaefb0ee2bac4bb8bc8"
  "348a5ec32737ef17089a706b318fb42ca03dea8fbdbb2b19df4344d27f87ae38"
  "b57ada496b51b799130e0a554a87c50d6675565044ba8a42fd12bf66fd9aa680"
  "a45bba02bdf8b2a5bd6da75af754b427694353c7ef864795b9a7e986350e1a23"
)

prepare() {
  cd "$srcdir"
  mv -T llvm-68584f0b7bc07d43af64f90b3726988b5a513bf9 hcc-roc-$pkgver/compiler
  mv -T compiler-rt-15e4a1f9195d3d90828a7a122d866c69e650155c hcc-roc-$pkgver/compiler-rt
  mv -T hcc-clang-upgrade-c792478f19beee13540053f188094898a008d245 hcc-roc-$pkgver/clang
  mv -T clang-tools-extra-b4a0f4adacc1dd5b2deecda2282b20af963716e7 hcc-roc-$pkgver/clang-tools-extra
  mv -T lld-8e7027a1bf3b2a0007562d2164e0fa4c037a31d5 hcc-roc-$pkgver/lld
  mv -T ROCm-Device-Libs-381a895a7e80d6ff0785faa21396e66c54d81f85 hcc-roc-$pkgver/rocdl
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
