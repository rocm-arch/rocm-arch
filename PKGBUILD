# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=3.0.0
pkgrel=1
_filename=roc-${pkgver}.tar.gz
pkgdesc="Utility to manage and monitor AMDGPU / ROCm systems."
arch=('any')
url="https://github.com/RadeonOpenCompute/ROC-smi/"
license=('MIT')
depends=(python)
source=(
  "$pkgname-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/ROC-smi/archive/${_filename}"
  "python2_fix.patch"
)
sha256sums=(
  'b35cb654c3a794586ff60cf226884239e19cb91c0a5c2cb1d4e7b1be1cb845b2'
  'SKIP'
)

prepare() {
	cd "${srcdir}/ROC-smi-roc-$pkgver"
	patch --forward --strip=1 --input="${srcdir}/python2_fix.patch"
}

package() {

    cd "ROC-smi-roc-${pkgver}"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/rocm-smi/LICENSE"
	install -Dm644 README.md "${pkgdir}/usr/share/doc/rocm-smi/README"

	install -Dm755 rocm_smi.py "${pkgdir}/usr/bin/rocm-smi"

}
