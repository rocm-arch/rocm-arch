# Maintainer: Ilya Elenskiy <elenskiy.ilya@gmail.com>
pkgname=hsa-amd-aqlprofile
pkgver=1.0.0
pkgrel=1
_debfile=hsa-amd-aqlprofile_1.0.0_amd64.deb 
pkgdesc="AQLPROFILE library for AMD HSA runtime API extension support"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/HSA-AqlProfile-AMD-extension"
license=('EULA')
groups=()
depends=(rocr-runtime)
makedepends=() 
replaces=()
backup=()
options=()
source=("http://repo.radeon.com/rocm/apt/debian/pool/main/h/hsa-amd-aqlprofile/${_debfile}")
sha256sums=('6640da8e5e641648c45453e7229b485727fb94f8d6733829f8c441c7cb29cdc2')

package() {
  cd "$srcdir"
  tar xf data.tar.gz
	
  cp -r opt $pkgdir/

  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/$pkgname.conf
      /opt/rocm/hsa-amd-aqlprofile/lib/
		EOF
}

