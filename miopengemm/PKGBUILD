# Maintainer: Torsten Keßler <t dot kessler at posteo dot de>
# Contributor: acxz <akashpatel2008 at yahoo dot com>
# Contributor: Jakub Okoński <jakub@okonski.org>
pkgname=miopengemm
pkgver=4.2.0
pkgrel=3
pkgdesc="An OpenCL GEMM kernel generator"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/MIOpenGEMM"
license=('MIT')
depends=('ocl-icd')
makedepends=('opencl-headers' 'cmake' 'ocl-icd' 'rocm-cmake')
source=("$pkgname-$pkgver.tar.gz::$url/archive/rocm-$pkgver.tar.gz"
        "fix-gcc11-build.patch::https://patch-diff.githubusercontent.com/raw/ROCmSoftwarePlatform/MIOpenGEMM/pull/46.patch")
sha256sums=('a11fa063248ed339fe897ab4c5d338b7279035fa37fcbe0909e2c4c352aaefb1'
            '5c81ee48b64de3c343cde2df8551bba966961b99d1fe4b12f0348ced5fe52a18')
_dirname="$(basename "$url")-$(basename "${source[0]}" ".tar.gz")"

prepare() {
    cd "$_dirname"
    # fix build with GCC 11, see https://github.com/ROCmSoftwarePlatform/rocBLAS/issues/1191#issuecomment-851696908
    patch -Np1 < "${srcdir}/fix-gcc11-build.patch"
}

build() {
  cmake -S "$_dirname" -Wno-dev \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm
  make
}

package() {
  make DESTDIR="$pkgdir" install

  install -Dm644 "$srcdir/$_dirname/LICENSE.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
