# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=hip-git
pkgver=r4264.0dadb233
pkgrel=1
pkgdesc="Heterogeneous Interface for Portability ROCm"
url="https://github.com/ROCm-Developer-Tools/HIP"
arch=(x86_64)
license=('Custom')
makedepends=(libelf git cmake gcc hcc)
provides=('hip')
conflicts=('hip')
_name=HIP
source=("git+https://github.com/ROCm-Developer-Tools/HIP.git")
sha256sums=("SKIP")

pkgver() {
  cd "$_name"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {

  cd "${srcdir}/${_name}"

  mkdir -p build

  cd "${srcdir}/${_name}/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" \
    ..

  make
}

package() {
  cd "${srcdir}/${_name}/build"

  make DESTDIR=${pkgdir} install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/hip.conf
    /opt/rocm/hip/lib/
		EOF

}
