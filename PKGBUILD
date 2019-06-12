# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocsparse
pkgver=2.5.0
pkgrel=1
pkgdesc="Next generation SPARSE implementation for ROCm platform."
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocSPARSE"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja hcc python2 rocminfo)
srcver="2.5"
source=("https://github.com/ROCmSoftwarePlatform/rocSPARSE/archive/rocm-$srcver.tar.gz")
sha256sums=("01f5eae7cdd9cb98f6ff765e053afd36dfab61a3217016420afbda8dc6ae8b35")

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

  # TODO: fix librocsparse.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocsparse" \
        -G Ninja \
        "$srcdir/rocSPARSE-rocm-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocsparse.conf
    /opt/rocm/rocsparse/lib/
		EOF
}
