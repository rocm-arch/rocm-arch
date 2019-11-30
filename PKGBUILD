# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=2.10.0
pkgrel=1
_filename=roc-${pkgver}.tar.gz
pkgdesc="Utility to manage and monitor AMDGPU / ROCm systems."
arch=('any')
url="https://github.com/RadeonOpenCompute/ROC-smi/"
license=('MIT')
depends=(python)
source=("$pkgname-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/ROC-smi/archive/${_filename}"
		"python2_fix.patch")
sha256sums=('8ed448491db59b0d586dff7475ec8c40b8fca2063c480f31c721f64b22a44326'
            '986f8021c0dc2c3bd8f2fa81f82c1c6cbe8e506e708b04840ea5d3c935f16832')

prepare() {
	cd "${srcdir}/ROC-smi-roc-${pkgver}"
	patch --forward --input="${srcdir}/python2_fix.patch"
}

package() {

    cd "ROC-smi-roc-${pkgver}"
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/rocm-smi/LICENSE"
	install -Dm644 README.md "${pkgdir}/usr/share/doc/rocm-smi/README"

	install -Dm755 rocm_smi.py "${pkgdir}/usr/bin/rocm-smi"

}
