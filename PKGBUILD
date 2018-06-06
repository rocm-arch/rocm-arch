# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsakmt-roct
pkgver=1.8.0.r0.e3dd067
pkgrel=2
pkgdesc="Radeon Open Compute Thunk Interface"
_gitdir=ROCT-Thunk-Interface
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
license=('X11/MIT')
groups=()
depends=(pciutils numactl)
makedepends=(git cmake gcc) 
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
source=('git+https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface.git#branch=roc-1.8.x')
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
	cmake -DCMAKE_INSTALL_PREFIX=/opt/rocm .. && \
	make
}

package() {
	cd "$srcdir/${_gitdir}/build"
	make DESTDIR="$pkgdir/" install

	# additional links
	mkdir -p "$pkgdir/usr/include"
	ln -s opt/rocm/libhsakmt/include/libhsakmt "$pkgdir/usr/include"

	# ldconfig
	mkdir -p "$pkgdir/etc/ld.so.conf.d"
	echo "/opt/rocm/libhsakmt/lib" > "$pkgdir/etc/ld.so.conf.d/libhsakmt.conf"

	############
	# dev-parts

	# pkg-build config file(s)
	find `pwd` -name "*.pc" -exec install -D '{}' $pkgdir/opt/rocm/libhsakmt ';'

	# headers, reconstruct tree
	cd "$srcdir/${_gitdir}/include"
	find . -name '*.h' -exec install -D '{}' $pkgdir/opt/rocm/libhsakmt/include/libhsakmt/{} ';'

	# cleanup
	rm -Rf "$pkgdir/opt/rocm/include"
	rm -Rf "$pkgdir/opt/rocm/lib"
}
