# Maintainer: Olaf Leidinger <oleid@mescharet.de>
pkgname=hsa-ext-rocr
pkgver=1.1.8
pkgrel=15
_debfile=hsa-ext-rocr-dev_${pkgver}-${pkgrel}-ge851b7a_amd64.deb
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
sha256sums=('35325126ca9f6318808c494b26dece65f93a5a0aba80604d0839a7f93d3b6fdb')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/usr/bin
	mkdir -p ${pkgdir}/usr/lib
	
	cp -ax opt/rocm/hsa/bin/* ${pkgdir}/usr/bin
	cp -ax opt/rocm/hsa/lib/* ${pkgdir}/usr/lib 
}
