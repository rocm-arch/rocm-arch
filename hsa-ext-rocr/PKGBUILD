# Maintainer: acxz <akashpatel2008 at yahoo dot com>
# Contributor: Bruno Filipe <bmilreu@gmail.com>
pkgname=hsa-ext-rocr
pkgver=1.1.30100
pkgrel=1
_debfile=hsa-ext-rocr-dev_1.1.30100.0-rocm-rel-3.1-44-ecafeba1_amd64.deb
pkgdesc="ROCm Platform Runtime: Closed source components"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm"
license=('EULA')
groups=()
depends=(hsa-rocr hsakmt-roct)
makedepends=()
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
source=("http://repo.radeon.com/rocm/apt/debian/pool/main/h/hsa-ext-rocr-dev/${_debfile}")
sha256sums=('fadff558ed486967ab048f6e393d13bfa9f1b480bcf37da9e41b44f4d99b218a')

package() {
	cd "$srcdir"
	tar xf data.tar.gz

	mkdir -p ${pkgdir}/usr/lib

	cp -ax opt/rocm-3.1.0/hsa/lib/* ${pkgdir}/usr/lib
}
