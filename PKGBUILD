# U-Boot for Raspberry Pi 3 armv7h
# Maintainer: Kevin Mihelich <kevin@archlinuxarm.org>

buildarch=12

pkgname=uboot-raspberrypi3-32b
pkgver=2018.07
pkgrel=1
pkgdesc="U-Boot for Raspberry Pi 3 armv7h"
arch=('armv7h')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr' 'boot/config.txt')
makedepends=('bc' 'dtc' 'git')
optdepends=('linux-raspberrypi: kernel for 32bit uboot')
conflicts=('uboot-raspberrypi')
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        'boot.txt.v3'
        'mkscr')
md5sums=('2b8eaa30dd118b29889669070da22bb0'
         'be8abe44b86d63428d7ac3acc64ee3bf'
         '021623a04afd29ac3f368977140cfbfd')

build() {
  cd u-boot-${pkgver/rc/-rc}

  unset CFLAGS
  unset CXXFLAGS
  unset CPPFLAGS

  make distclean
  make rpi_3_32b_config
  echo 'CONFIG_IDENT_STRING=" Arch Linux ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}"/boot

  cp u-boot.bin ${pkgdir}/boot/u-boot.img
  cp ../boot.txt.v3 ../boot.txt
  echo -e "kernel=u-boot.img\nenable_uart=1\nforce_turbo=1" > ${pkgdir}/boot/config.txt

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}"/boot/boot.scr
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
