# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocthrust
pkgver=3.0.0
pkgrel=1
pkgdesc="Port of the Thrust parallel algorithm library atop HIP/ROCm."
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocThrust"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc ninja hcc python2 rocminfo)
srcver="2.7"
source=("https://github.com/ROCmSoftwarePlatform/rocThrust/archive/rocm-$srcver.tar.gz")
sha256sums=("19ac24023a1015df19b29f3a8afcd61113ed91b26a7d3fa33b00a1f742e51be1")

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
        -DBUILD_TEST=OFF \
        -G Ninja \
        "$srcdir/rocThrust-rocm-$srcver"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocthrust.conf
    /opt/rocm/rocthrust/lib/
		EOF
}
