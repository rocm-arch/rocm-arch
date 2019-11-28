# Original build: fermyon <antifermion@protonmail.com>
# Maintainer: Markus Näther <naetherm@cs.uni-freiburg.de>
pkgname=rocm-cmake
pkgver=2.10.0
pkgrel=1
pkgdesc="cmake modules for common build tasks needed for the ROCM software stack"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/rocm-cmake"
license=(MIT)
makedepends=(git cmake)
conflicts=(rocm-cmake-git)
source=("https://github.com/RadeonOpenCompute/rocm-cmake/archive/roc-$pkgver.tar.gz")
sha256sums=('4ff9db5cf1cee1f4af96d0240e1a3068b39111b559808e6437f3748f86673181')

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
