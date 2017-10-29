# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsakmt-roct
pkgver=1.6.3.r0.25a9bc2
pkgrel=1
pkgdesc="Radeon Open Compute Thunk Interface"
_gitdir=ROCT-Thunk-Interface
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
license=('X11/MIT')
groups=()
depends=(pciutils)
makedepends=(git cmake gcc) 
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
	
	# build fix for gcc-7.2 from drm-next-wip branch
	git cherry-pick f13442e0a9ff117e6610d3f93b198082b95942bb
	
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

	# cleanup
	rm -Rf "$pkgdir/opt/rocm/include"
	rm -Rf "$pkgdir/opt/rocm/lib"
}
