# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=1.9.2
pkgrel=1
_filename=roc-${pkgver}.tar.gz
pkgdesc="Includes the rocm-smi tool. This tool exposes functionality for clock and temperature management of AMD ROCm enabled systems."
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROC-smi/"
license=('MIT')
groups=()
depends=(python)
makedepends=()
provides=("${pkgname}")
conflicts=("${pkgname}")
replaces=()
backup=()
options=()
source=("$pkgname-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/ROC-smi/archive/${_filename}")
sha256sums=('14388908953798b3ea761011e3ef972a47c9ae1e253b651f4056c3161ae86f30')

package() {

	tar xf "${srcdir}/${pkgname}-${pkgver}.tar.gz" --strip-components=1

	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/rocm-smi/LICENSE"
	install -Dm644 README.md "${pkgdir}/usr/share/doc/rocm-smi/README"

	install -Dm755 rocm_smi.py "${pkgdir}/usr/bin/rocm-smi"
	#install -Dm644 test-rocm-smi.sh "${pkgdir}/usr/bin/test-rocm-smi"

}
