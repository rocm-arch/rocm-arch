# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=rocm-utils
pkgver=1.0.0
pkgrel=1
_debfile=rocm-utils_${pkgver}_amd64.deb
pkgdesc="ROCm Platform Runtime: Closed source utils"
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
source=("http://repo.radeon.com/rocm/apt/debian/pool/main/r/rocm-utils/${_debfile}")
sha256sums=('5f172015945688575615af307fa312c21c72551b479647b4dbf150770f4d93bc')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/usr/bin
	
	cp -ax opt/rocm/bin/* ${pkgdir}/usr/bin
}
