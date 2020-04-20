# Maintainer: Ranieri Althoff <ranisalt+aur at gmail dot com>

_opencl_icd_loader_repo='https://github.com/KhronosGroup/OpenCL-ICD-Loader'
_opencl_icd_loader_commit='978b4b3a29a3aebc86ce9315d5c5963e88722d03'

pkgname=rocm-opencl-runtime
pkgver=3.3.0
pkgrel=2
pkgdesc='Radeon Open Compute - OpenCL runtime'
arch=('x86_64')
url='https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime'
license=('MIT')
depends=('hsakmt-roct' 'hsa-rocr' 'opencl-icd-loader')
makedepends=('mesa' 'cmake' 'git' 'llvm-roc' 'rocm-cmake' 'rocm-comgr')
provides=("$pkgname" 'opencl-driver')
source=("$url/archive/roc-$pkgver.tar.gz"
        "$_opencl_icd_loader_repo/archive/$_opencl_icd_loader_commit.tar.gz")
sha256sums=('ac6999f1a491ab066286c2bd6adf50f08f831286f56e267879f9f7eced22f98e'
            '0c14bf890bd198ef5a814b5b7ed57b69e890b0c0a1bcfba8fdad996fa1a97fc7')
_dirname="$(basename "$url")-$(basename "${source[0]}" .tar.gz)"
_opencl_dirname="$(basename "$_opencl_icd_loader_repo")-$(basename "${source[1]}" .tar.gz)"

prepare() {
    mkdir -p "$_dirname/api/opencl/khronos"
    mv "$_opencl_dirname" "$_dirname/api/opencl/khronos/icd"
}

build() {
    cmake -DCMAKE_INSTALL_PREFIX=/opt/rocm \
          -DCMAKE_INSTALL_SYSCONFDIR=/etc \
          -DCMAKE_MODULE_PATH=/opt/rocm/share/rocm/cmake \
          -DCMAKE_PREFIX_PATH=/opt/rocm/lib/cmake \
          -DUSE_COMGR_LIBRARY=yes \
          "$_dirname"
    make
}

package() {
    DESTDIR="$pkgdir" make install

    mkdir -p "$pkgdir/etc/ld.so.conf.d"
    echo /opt/rocm/lib > "$pkgdir/etc/ld.so.conf.d/$pkgname.conf"

    install -Dm644 "$_dirname/License" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
