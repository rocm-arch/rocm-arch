# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsa-rocr
pkgver=1.1.0.r18.8d56c60
pkgrel=1
pkgdesc="ROCm Platform Runtime: ROCr a HPC market enhanced HSA based runtime"
_gitdir=ROCR-Runtime
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
license=('X11/MIT')
groups=()
depends=()
makedepends=(git cmake gcc hsakmt-roct) 
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
source=('git+https://github.com/RadeonOpenCompute/ROCR-Runtime.git#branch=roc-1.6.x')
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
	cmake -DCMAKE_INSTALL_PREFIX=/opt/rocm -DCMAKE_PREFIX_PATH=/opt/rocm/libhsakmt ../src && \
	make
}

package() {
	cd "$srcdir/${_gitdir}/build"
	make DESTDIR="$pkgdir/" install

	# additional links
	mkdir -p "$pkgdir/usr/include"
	ln -s opt/rocm/hsa/include/hsa "$pkgdir/usr/include"

	# ldconfig
	mkdir -p "$pkgdir/etc/ld.so.conf.d"
	echo "/opt/rocm/hsa/lib" > "$pkgdir/etc/ld.so.conf.d/hsa.conf"

	# cleanup
	rm -Rf "$pkgdir/opt/rocm/include"
	rm -Rf "$pkgdir/opt/rocm/lib"
}
