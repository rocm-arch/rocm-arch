# Maintainer: acxz <akashpatel2008 at yahoo dot com>
pkgname=mivisionx
pkgver=1.6
pkgrel=1
pkgdesc="Set of comprehensive computer vision and machine intelligence
libraries, utilities, and applications bundled into a single toolkit"
arch=('x86_64')
url="https://gpuopen-professionalcompute-libraries.github.io/MIVisionX/"
license=('MIT')
depends=('rocm' 'rocm-cmake' 'miopengemm' 'miopen' 'protobuf' 'opencv' 'ffmpeg')
makedepends=('cmake')
source=("mivisionx-$pkgver.tar.gz::https://github.com/GPUOpen-ProfessionalCompute-Libraries/MIVisionX/archive/$pkgver.tar.gz")
sha256sums=('1dee647ef476ac7d7c960f7923d2598716d1b6043608c4349832d6a54ba1da1b')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/mivisionx \
        "$srcdir/mivisionx-$pkgver"
  make
}

package() {
  cd "$srcdir/build"

  make DESTDIR="$pkgdir" install

  # add links
  install -d "$pkgdir/usr/bin"
  local _fn
  for _fn in mivisionx; do
    ln -s "/opt/rocm/mivisionx/bin/$_fn" "$pkgdir/usr/bin/$_fn"
  done
}
