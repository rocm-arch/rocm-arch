# Maintainer: Bruno Filipe <bmilreu@gmail.com>
pkgname=hsa-ext-rocr
pkgver=1.1.9
pkgrel=15
_debfile=hsa-ext-rocr-dev_1.1.9-15-g848be2f_amd64.deb
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
sha256sums=('24a2fe4afb43263ed10469c1279f6dd6261932a9a8aaaa5911c8ece8af06f5a4')

package() {
	cd "$srcdir"
	tar xf data.tar.gz
	
	mkdir -p ${pkgdir}/usr/lib
	
	cp -ax opt/rocm/hsa/lib/* ${pkgdir}/usr/lib 
}
