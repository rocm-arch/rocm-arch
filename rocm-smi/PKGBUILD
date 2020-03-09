# Maintainer: Rigo Reddig <rigo.reddig@gmail.com>
pkgname=rocm-smi
pkgver=3.1.0
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
sha256sums=('df8a1f646ad0bfa6c8a0a0545b55e79514f22319df92a2d8290fd4aa58aaaf61'
            'f49b7035dfd9bee2bafb633c3b54e011bbb05d3b5a495937d655ef0fd377b462')

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
