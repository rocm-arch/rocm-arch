# Maintainer: Jakub Okoński <jakub@okonski.org>
# Maintainer: Markus Näther <naetherm@cs.uni-freiburg.de>
pkgname=rocfft
pkgver=3.0.0
pkgrel=1
pkgdesc="Next generation FFT implementation for ROCm"
url="https://github.com/ROCmSoftwarePlatform/rocfft"
arch=(x86_64)
depends=(boost fftw)
_srcver="3.0"
makedepends=("hcc>=3.0.0" git cmake ninja)
source=("https://github.com/ROCmSoftwarePlatform/rocFFT/archive/rocm-$_srcver.tar.gz")
sha256sums=("deadd7890df3c9350cbf7728708e620191e7cbc681d091004eee099186fb4e10")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # build broken with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  cmake -DCMAKE_BUILD_TYPE=Release \
        -G Ninja \
        "$srcdir/rocFFT-rocm-$_srcver"

  ninja
}

package() {
  DESTDIR="$pkgdir" ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocfft.conf
    /opt/rocm/rocfft/lib
		EOF
}
