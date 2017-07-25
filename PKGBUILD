# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsa-ext-rocr
pkgver=1.1.6
pkgrel=33
_debfile=hsa-ext-rocr-dev_${pkgver}-${pkgrel}-g9f17990_amd64.deb
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
sha256sums=('0b78e91459fdc9cf19945e1a6e66bb578c6b00568e365a469a74827c3c8c6a61')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/usr/bin
	mkdir -p ${pkgdir}/usr/lib
	
	cp -ax opt/rocm/hsa/bin/* ${pkgdir}/usr/bin
	cp -ax opt/rocm/hsa/lib/* ${pkgdir}/usr/lib 
}
