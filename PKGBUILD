# Submitter: Olaf Leidinger <oleid@mescharet.de>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hcc
pkgver=2.1.0
pkgrel=2
pkgdesc="HCC is an Open Source, Optimizing C++ Compiler for Heterogeneous Compute"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/hcc"
license=('NCSAOSL')
depends=(rocr-runtime z3)
makedepends=(git cmake gcc ninja z3)
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
options=(!staticlibs strip)
source=("git+https://github.com/RadeonOpenCompute/hcc.git#tag=roc-2.1.0")
md5sums=('SKIP')

prepare() {
  cd "$srcdir/hcc"
  git submodule update --init --progress
}

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hcc" \
        -G Ninja \
        "$srcdir/hcc"
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
