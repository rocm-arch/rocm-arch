# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocblas
pkgver=2.2.0
pkgrel=1
pkgdesc="Next generation BLAS implementation for ROCm platform"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocBLAS"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja hcc python2 rocminfo)
source=("https://github.com/ROCmSoftwarePlatform/rocBLAS/archive/v$pkgver.tar.gz")
sha256sums=("b981257feb28faf70c1681ae6dd330b0ab1b687bc6da165f266be3d26f4718fe")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # Tensile library needs python to be python2
  export PATH="$srcdir:$PATH"
  [[ -e "$srcdir/python" ]] || ln -s /usr/bin/python2 "$srcdir/python"

  # fix broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  # TODO: fix librocblas.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocblas" \
        -G Ninja \
        "$srcdir/rocBLAS-$pkgver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocblas.conf
    /opt/rocm/rocblas/lib/
		EOF
}

