# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocthrust
pkgver=2.6.0
pkgrel=1
pkgdesc="Port of the Thrust parallel algorithm library atop HIP/ROCm."
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocThrust"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja hcc python2 rocminfo)
srcver="2.6.0"
source=("https://github.com/ROCmSoftwarePlatform/rocThrust/archive/$srcver.tar.gz")
sha256sums=("1252f56cc91040d5be7a816a9e3623ba968c40ef61706173813fcf2af4832d7e")

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

  # TODO: fix librocthrust.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocthrust" \
        -G Ninja \
        "$srcdir/rocThrust-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocthrust.conf
    /opt/rocm/rocthrust/lib/
		EOF
}
