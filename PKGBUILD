# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=1.9.2
pkgrel=2
_filename=roc-${pkgver}.tar.gz
_subdir=ROC-smi-roc-${pkgver}
pkgdesc="Utility to manage and monitor AMDGPU / ROCm systems."
arch=('any')
url="https://github.com/RadeonOpenCompute/ROC-smi/"
license=('MIT')
depends=(python)
source=("$pkgname-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/ROC-smi/archive/${_filename}")
sha256sums=('14388908953798b3ea761011e3ef972a47c9ae1e253b651f4056c3161ae86f30')

package() {

	install -Dm644 "${_subdir}/LICENSE" "${pkgdir}/usr/share/licenses/rocm-smi/LICENSE"
	install -Dm644 "${_subdir}/README.md" "${pkgdir}/usr/share/doc/rocm-smi/README"

	install -Dm755 "${_subdir}/rocm_smi.py" "${pkgdir}/usr/bin/rocm-smi"

}
