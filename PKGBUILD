# Original build: fermyon <antifermion@protonmail.com>
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocm-cmake-git
pkgver=r64.ac45c6e
pkgrel=1
pkgdesc="cmake modules for common build tasks needed for the ROCM software stack"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/rocm-cmake"
license=(MIT)
makedepends=(git cmake)
source=('git+https://github.com/RadeonOpenCompute/rocm-cmake.git#branch=master')
md5sums=('SKIP')

pkgver() {
	cd "$srcdir/${pkgname%-git}"
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
	cd "$srcdir/${pkgname%-git}"
	mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" ..
}

package() {
	cd "$srcdir/${pkgname%-git}/build"
    cmake --build . --target install
}
