# Original build: fermyon <antifermion@protonmail.com>
# Maintainer: Markus Näther <naetherm@cs.uni-freiburg.de>
pkgname=rocm-cmake
pkgver=3.0.0
pkgrel=1
pkgdesc="cmake modules for common build tasks needed for the ROCM software stack"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/rocm-cmake"
license=(MIT)
makedepends=(git cmake)
conflicts=(rocm-cmake-git)
source=("https://github.com/RadeonOpenCompute/rocm-cmake/archive/roc-$pkgver.tar.gz")
sha256sums=('9393fd534a100e8da55f44d0625337d76f3c7890e0297fe690c9e7d384b205ef')

build() {
	cd "$srcdir/${pkgname}-roc-${pkgver}"
	mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" ..
}

package() {
	cd "$srcdir/${pkgname}-roc-${pkgver}/build"
    cmake --build . --target install
}
