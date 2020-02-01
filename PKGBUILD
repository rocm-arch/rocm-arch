# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rccl
pkgver=3.0.0
pkgrel=1
pkgdesc="ROCm Communication Collectives Library"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rccl"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc make hcc python2 rocminfo)
srcver="3.0.0"
source=("https://github.com/ROCmSoftwarePlatform/rccl/archive/$srcver.tar.gz")
sha256sums=("55b71a8a3040ab0b6c5750eefa10539ecd7310ed010775e5040ad459b3f48a25")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # fix broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  # TODO: fix librccl.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rccl" \
        -DBUILD_TESTS=OFF \
        -G "Unix Makefiles" \
        "$srcdir/rccl-$srcver"
  make 
}

package() {
  cd $srcdir/build
  make install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rccl.conf
    /opt/rocm/rccl/lib/
		EOF
}
