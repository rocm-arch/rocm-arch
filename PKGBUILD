# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=roct-thunk-interface
pkgver=2.5.0
pkgrel=1
pkgdesc="ROCm HSA"
arch=(x86_64)
url="https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface"
license=('unknown')
makedepends=(git cmake gcc ninja)
depends=(numactl pciutils)
source=("https://github.com/RadeonOpenCompute/ROCT-Thunk-Interface/archive/roc-$pkgver.tar.gz" "fix_build-dev_command.patch")
sha256sums=(
  "1ec04105c9569040c7bdd1a7513c17a0c4d67d31526c4bf33394842ebd482167"
  "f58bdd6df3065ad1e08c107e95140445fc01bd46dae27fb9d2091a2f3b5d3d2f"
)

prepare() {
    cd ROCT-Thunk-Interface-roc-$pkgver
    patch -Np1 -i "$srcdir/fix_build-dev_command.patch"
}

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm" \
        -G Ninja \
        "$srcdir/ROCT-Thunk-Interface-roc-$pkgver"
  ninja all build-dev
}

package() {
  ninja -C "$srcdir/build" install install-dev
  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/roct-thunk-interface.conf
		/opt/rocm/lib64
		EOF
}
