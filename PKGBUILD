# U-Boot: Turing RK1
# Maintainer: Damien FLETY <damfle+archlinux@sloth.ninja>

buildarch=8

pkgname=uboot-rk1
pkgver=2025.01
pkgrel=1
pkgdesc="U-Boot for TuringPi RK1"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/uboot.env')
makedepends=('bc' 'git' 'python' 'python-setuptools' 'python-pyelftools' 'swig' 'dtc' 'uboot-tools')
_srcname=u-boot
_gitbranch=v2025.01
_rkbin_version=f43a462e7a1429a9d407ae52b4745033034a6cf9
source=(
        "${_srcname}::git+https://github.com/u-boot/u-boot#tag=${_gitbranch}"
        "rkbin::git+https://github.com/rockchip-linux/rkbin#commit=${_rkbin_version}"
        "config"
        "99-uboot-post.sh"
        "flash_uboot.sh"
      )
sha256sums=('SKIP'
            'SKIP'
            '8ed6f182633c210b7302f03a68523819ef838ee36891d3037d03ce4c836cb365'
            '516f02c00926ee90e611261848c8711d24e6026d5f8483af6ae7759ce4513011'
            'f9c325600b40ebc549cffdb45d090652de170db9664d7292904cb8c6d0acf1bb'
      )

build() {
  export ROCKCHIP_TPL="${srcdir}/rkbin/bin/rk35/rk3588_ddr_lp4_2112MHz_lp5_2400MHz_v1.18.bin"
  export BL31="${srcdir}/rkbin/bin/rk35/rk3588_bl31_v1.48.elf"

  cp config "${srcdir}/u-boot/.config"

  cd "${srcdir}/u-boot"

  unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

  make oldconfig
  make
}

package() {
  sed "${_subst}" "${srcdir}/99-uboot-post.sh" |
    install -Dm744 /dev/stdin "${pkgdir}/etc/initcpio/post/99-uboot-post.sh"

  sed "${_subst}" "${srcdir}/flash_uboot.sh" |
    install -Dm744 /dev/stdin "${pkgdir}/boot/flash_uboot.sh"

  sed "${_subst}" "${srcdir}/u-boot/u-boot-rockchip.bin" |
    install -Dm744 /dev/stdin "${pkgdir}/boot/u-boot-rockchip.bin"
}

