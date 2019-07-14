# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rccl
pkgver=2.6.0
pkgrel=1
pkgdesc="ROCm Communication Collectives Library"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rccl"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc make hcc python2 rocminfo)
srcver="2.6.0"
source=("https://github.com/ROCmSoftwarePlatform/rccl/archive/$srcver.tar.gz")
sha256sums=("2be14e53a96769cf37a7cb4f933d580736dbcdf0051fa4a327257e5c017fd445")

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
        -DBUILD_TEST=OFF \
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
