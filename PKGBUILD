# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=rocm-utils
pkgver=1.8.151
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
sha256sums=('759b9d9965df927d3a7e0452aa1083608e3286849e9c9b769526961f8628b24f')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/opt/rocm/bin
	
	cp -ax opt/rocm/bin/* ${pkgdir}/opt/rocm/bin
}
