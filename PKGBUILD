# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=rocm-utils
pkgver=1.0.0
pkgrel=3
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
sha256sums=('e2feebcdd061f4bbdfb8b38210104510d0967099465de778190ab0fc66bf1c95')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/opt/rocm/bin
	
	cp -ax opt/rocm/bin/* ${pkgdir}/opt/rocm/bin
}
