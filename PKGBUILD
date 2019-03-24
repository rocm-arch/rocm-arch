# Maintainer: Ilya Elenskiy <elenskiy.ilya@gmail.com>
pkgname=rocm-profiler
_branch=2.2.x
pkgver=2.2.0
pkgrel=2
pkgdesc="ROC profiler library. Profiling with perf-counters and derived metrics for GFX8/GFX9."
arch=(x86_64)
url="https://github.com/ROCm-Developer-Tools/rocprofiler"
license=('MIT')
makedepends=(git cmake gcc ninja)
depends=('rocr-runtime>=2.2.0' 'hsa-amd-aqlprofile')
source=("rocprofiler::git+https://github.com/ROCm-Developer-Tools/rocprofiler.git#branch=roc-$_branch")
sha256sums=('SKIP')

build() {
  mkdir -p "$srcdir/rocprofiler/build"
  cd "$srcdir/rocprofiler/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=$pkgdir/opt/rocm \
        -DCMAKE_PREFIX_PATH=/opt/rocm \
        -G Ninja \
        ..
  ninja
}

check() {
  cd "$srcdir/rocprofiler/build"
  ./run.sh
}

package() {
  ninja -C "$srcdir/rocprofiler/build" install

  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocprofiler.conf
      /opt/rocm/rocprofiler/lib/
		EOF
}
