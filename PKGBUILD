# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsa-ext-rocr
pkgver=1.1.6
pkgrel=35
_debfile=hsa-ext-rocr-dev_${pkgver}-${pkgrel}-g624bb41_amd64.deb
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
sha256sums=('f04be739ff3d3757933188145be3fc0cce45e243d70f7fa92a33e4691166277e')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/usr/bin
	mkdir -p ${pkgdir}/usr/lib
	
	cp -ax opt/rocm/hsa/bin/* ${pkgdir}/usr/bin
	cp -ax opt/rocm/hsa/lib/* ${pkgdir}/usr/lib 
}
