# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=2.2.0
pkgrel=1
_filename=roc-${pkgver}.tar.gz
pkgdesc="Utility to manage and monitor AMDGPU / ROCm systems."
arch=('any')
url="https://github.com/RadeonOpenCompute/ROC-smi/"
license=('MIT')
depends=(python)
source=("$pkgname-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/ROC-smi/archive/${_filename}")
sha256sums=('08e48412eb4b737cdbde046bceff2ab6efb2bd6c6614ab7886ad962ffbf5b5fa')

package() {

    cd "ROC-smi-roc-${pkgver}"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/rocm-smi/LICENSE"
	install -Dm644 README.md "${pkgdir}/usr/share/doc/rocm-smi/README"

	install -Dm755 rocm_smi.py "${pkgdir}/usr/bin/rocm-smi"

}
