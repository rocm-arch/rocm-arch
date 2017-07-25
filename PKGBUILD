# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsakmt-roct
pkgver=1.6.1.r0.a65d730
pkgrel=1
pkgdesc="Radeon Open Compute Thunk Interface"
_gitdir=ROCT-Thunk-Interface
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
license=('X11/MIT')
groups=()
depends=()
makedepends=('git cmake gcc') 
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
source=('git+https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface.git#branch=roc-1.6.x')
md5sums=('SKIP')

pkgver() {
    cd "$srcdir/${_gitdir}"

# Git, tags available
	printf "%s" "$(git describe --long | sed -e 's:roc-::g'  -e 's/\([^-]*-\)g/r\1/;s/-/./g')"
}

build() {
	cd "$srcdir/${_gitdir}"
	mkdir -p build && \
	cd build && \
	cmake -DCMAKE_INSTALL_PREFIX=/usr .. && \
	make
}

package() {
	cd "$srcdir/${_gitdir}/build"
	make DESTDIR="$pkgdir/" install
}
