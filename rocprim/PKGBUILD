# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocprim
pkgver=3.0.0
pkgrel=1
pkgdesc="ROCm Parallel Primitives"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocPRIM"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc make hcc python2 rocminfo)
srcver="3.0.0"
source=("https://github.com/ROCmSoftwarePlatform/rocPRIM/archive/$srcver.tar.gz")
sha256sums=("cf3b14b700795c238cf0a0ab4dedca092cdc83f797e4c6331fba33a9f6358526")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # fix broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  # TODO: fix librocprim.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocprim" \
        -DBUILD_TEST=OFF \
        -G Ninja \
        "$srcdir/rocPRIM-$srcver"
  ninja 
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocprim.conf
    /opt/rocm/rocprim/lib/
		EOF
}
