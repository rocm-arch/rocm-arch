# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hcc
pkgver=1.0.17236.r102.g9b43ce89
pkgrel=1
pkgdesc="HCC is an Open Source, Optimizing C++ Compiler for Heterogeneous Compute"
_gitdir=hcc
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/hcc"
license=('NCSAOSL')
groups=()
depends=(rocm-utils hsakmt-roct hsa-rocr hsa-ext-rocr)
makedepends=(git cmake gcc) 
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=(!staticlibs strip)
source=('git+https://github.com/RadeonOpenCompute/hcc.git#branch=clang_tot_upgrade')
md5sums=('SKIP')

pkgver() {
    cd "$srcdir/${_gitdir}"

    git describe --long --tags | sed 's/^preview_//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "$srcdir/${_gitdir}"
    git submodule init
    git submodule update
}

build() {
	cd "$srcdir/${_gitdir}"
	mkdir -p build
	cd build
	cmake \
	    -DCMAKE_BUILD_TYPE=Release \
	    -DAMDHSACOD=/usr/bin/amdhsacod \
	    -DHSA_HEADER=/opt/rocm/hsa/include \
	    -DHSA_LIBRARY=/opt/rocm/hsa/lib/libhsa-runtime64.so.1 \
	    ..
}

package() {
	cd "$srcdir/${_gitdir}/build"
	env DESTDIR="$pkgdir/" make install
	
	# add links
	mkdir -p "$pkgdir/usr/bin"
	for fn in hcc hcc-config
	do
	    ln -s /opt/rocm/hcc-1.0/bin/$fn "$pkgdir/usr/bin/$fn"
	done
}
