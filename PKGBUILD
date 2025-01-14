# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Andrea Scarpino <andrea@archlinux.org>

originalpkgname=sddm
pkgname=sddm-BeiyanYunyi
pkgver=0.21.0
pkgrel=6
pkgdesc='QML based X11 and Wayland display manager'
arch=(x86_64)
url='https://github.com/sddm/sddm'
license=(GPL-2.0-only)
depends=(bash
         gcc-libs
         glibc
         libxau
         libxcb
         pam
         qt6-base
         qt6-declarative
         systemd-libs
         ttf-font
         xorg-server
         xorg-xauth)
makedepends=(extra-cmake-modules
             python-docutils
             qt5-base
             qt5-declarative
             qt5-tools
             qt6-tools)
optdepends=('qt5-declarative: for using Qt5 themes')
backup=('usr/share/sddm/scripts/Xsetup'
        'usr/share/sddm/scripts/Xstop'
        'etc/pam.d/sddm'
        'etc/pam.d/sddm-autologin'
        'etc/pam.d/sddm-greeter')
conflicts=(sddm)
provides=(display-manager sddm)
source=(https://github.com/$originalpkgname/$originalpkgname/archive/v$pkgver/$originalpkgname-$pkgver.tar.gz
        https://github.com/sddm/sddm/pull/1779.patch)
sha256sums=('f895de2683627e969e4849dbfbbb2b500787481ca5ba0de6d6dfdae5f1549abf'
            '7cce46811559bdeeb465f278f9b4736cca8a658d1df6f858af5b7a70efbdd5aa')


prepare() {
  patch -d $originalpkgname-$pkgver -Np1 -i ../1779.patch
}

build() {
  cmake -B build -S $originalpkgname-$pkgver \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBEXECDIR=/usr/lib/sddm \
        -DBUILD_WITH_QT6=ON \
        -DDBUS_CONFIG_DIR=/usr/share/dbus-1/system.d \
        -DDBUS_CONFIG_FILENAME=sddm_org.freedesktop.DisplayManager.conf \
        -DBUILD_MAN_PAGES=ON \
        -DUID_MAX=60513
  cmake --build build

  cmake -B build5 -S $originalpkgname-$pkgver \
        -DCMAKE_INSTALL_PREFIX=/usr
  cmake --build build5/src/greeter
  cmake --build build5/components
}

package() {
  DESTDIR="$pkgdir" cmake --install build
  DESTDIR="$pkgdir" cmake --install build5/src/greeter
  DESTDIR="$pkgdir" cmake --install build5/components

  install -d "$pkgdir"/usr/lib/sddm/sddm.conf.d
  "$pkgdir"/usr/bin/sddm --example-config > "$pkgdir"/usr/lib/sddm/sddm.conf.d/default.conf
# Don't set PATH in sddm.conf
  sed -r 's|DefaultPath=.*|DefaultPath=/usr/local/sbin:/usr/local/bin:/usr/bin|g' -i "$pkgdir"/usr/lib/sddm/sddm.conf.d/default.conf
# Unset InputMethod https://github.com/sddm/sddm/issues/952
  sed -e "/^InputMethod/s/qtvirtualkeyboard//" -i "$pkgdir"/usr/lib/sddm/sddm.conf.d/default.conf
}
