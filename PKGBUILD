# Maintainer: Lucas Magalhães <whoisroot@national.shitposting.agency>
pkgname=rocm-clang-ocl
pkgver=0.4.0
_buildrel=7ce124f
pkgrel=1
_debfile=rocm-clang-ocl_${pkgver}-${_buildrel}_amd64.deb
pkgdesc="OpenCL compilation with clang compiler."
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm"
license=('EULA')
groups=()
depends=()
makedepends=() 
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
source=("http://repo.radeon.com/rocm/apt/debian/pool/main/r/rocm-clang-ocl/${_debfile}")
sha256sums=('c329f2dc85675ae60dc30c9d4749b7620029c3857a48c443c80ec98a19e687d5')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/opt/rocm/bin
	
	cp -ax opt/rocm/bin/* ${pkgdir}/opt/rocm/bin
}
