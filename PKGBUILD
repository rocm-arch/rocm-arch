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
sha256sums=('948ec2045681160de795570ee476049d53565821e0ffe245ac2e52e0d21a5b6a')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/opt/rocm/bin
	
	cp -ax opt/rocm/bin/* ${pkgdir}/opt/rocm/bin
}
