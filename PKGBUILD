# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=2.0.0
pkgrel=1
_filename=roc-${pkgver}.tar.gz
pkgdesc="Utility to manage and monitor AMDGPU / ROCm systems."
arch=('any')
url="https://github.com/RadeonOpenCompute/ROC-smi/"
license=('MIT')
depends=(python)
source=("$pkgname-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/ROC-smi/archive/${_filename}")
sha256sums=('47b84798c95724708346ab505662a05a9a16e0fa0129b4371c073939d1c9fd35')

package() {

    cd "ROC-smi-roc-${pkgver}"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/rocm-smi/LICENSE"
	install -Dm644 README.md "${pkgdir}/usr/share/doc/rocm-smi/README"

	install -Dm755 rocm_smi.py "${pkgdir}/usr/bin/rocm-smi"

}
