# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsa-ext-rocr
pkgver=1.1.6
pkgrel=85
_debfile=hsa-ext-rocr-dev_${pkgver}-${pkgrel}-g466237c_amd64.deb
pkgdesc="ROCm Platform Runtime: Closed source components"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm"
license=('EULA')
groups=()
depends=(hsa-rocr)
makedepends=() 
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
source=("http://repo.radeon.com/rocm/apt/debian/pool/main/h/hsa-ext-rocr-dev/${_debfile}")
sha256sums=('b5dd84331ea62b26184a808e788c8a2ace86086bcf7d7ae6104109cb03d3d1da')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/usr/bin
	mkdir -p ${pkgdir}/usr/lib
	
	cp -ax opt/rocm/hsa/bin/* ${pkgdir}/usr/bin
	cp -ax opt/rocm/hsa/lib/* ${pkgdir}/usr/lib 
}
