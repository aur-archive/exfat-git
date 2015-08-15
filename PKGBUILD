# Maintainer: rtfreedman  <rob<d0t>til<d0t>freedman< T>googlemail<d0t>com>
#
# This source is now officially GPL but still (software) patent encumbered.
# Using it might be a problem in your country - in most others it isn't ;)
#
# If you're unsure, use it only on your own private systems
# and not inside your company or systems facing the public.
#
# This PKGBUILD will be available as long as the repository is public.
# http://opensource.samsung.com/
# 

pkgname=exfat-git
pkgver=133.97229de
pkgrel=1
url='https://github.com/dorimanx/exfat-nofuse.git'
pkgdesc='Native (nofuse) kernel module for EXtendedFAT support'
license=('GPL2')
#
arch=('i686' 'x86_64')
depends=('linux>=3.0')
makedepends=('linux-headers' 'git')
provides=('exfat')
conflicts=('exfat')
#
install="${pkgname}.install"
options=('!strip')
source=("${pkgname}::git+https://github.com/dorimanx/exfat-nofuse.git")
md5sums=('SKIP')

pkgver() {
  cd "${pkgname}"
  echo $(git rev-list --count master).$(git rev-parse --short master)
}

build() {
  cd "${pkgname}"
  # avoid confusing error message
  sed -i 's/^KREL/#KREL/' Makefile
  for _kernel in $(find /usr/lib/modules/extramodules* -type f -name version -printf '%h\n'); do
    _kdir=/usr/lib/modules/$(cat $_kernel/version)/build
    make KDIR=$_kdir
    mkdir -p "${srcdir}"/modules/$_kernel
    gzip -c exfat.ko > "${srcdir}"/modules/$_kernel/exfat.ko.gz
    make KDIR=$_kdir clean 
  done
}

package() {
  cd modules
  for _module in `find . -type f`; do
    install -m644 -D $_module "${pkgdir}"/$_module
  done
}
