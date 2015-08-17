# $Id$
# Maintainer of wicd PKGBUILD: Daniel Isenmann <daniel@archlinux.org>
# Modified by: Noctivivans <noctivivans@gmail.com>

pkgname=wicd-ipv6
_pkgname=wicd
pkgver=1.7.2.4
pkgrel=2
arch=(any)
url="http://wicd.sourceforge.net/"
license=('GPL2')
conflicts=('wicd' 'wicd-svn')
provides=('wicd')
replaces=('wicd' 'wicd-svn')
install=wicd.install
source=(http://launchpad.net/wicd/1.7/$pkgver/+download/wicd-$pkgver.tar.gz
	support.ipv6.dns.patch
	wicd-daemon
	wicd.install)
makedepends=('python2' 'python2-babel' 'python2-distribute')
options=('emptydirs')
md5sums=('c2435ddfdef0b9898852d72a85a45f0f'
	'b3d630f5b18cf7569175d381fc6fd3df'
        'f40e5f59998d0829707a7c9976afa8f8'
	'653f3ced93650e266ec3d9b0ca630c8a')
build() {
  cd $srcdir/$_pkgname-$pkgver
  patch -p1 -i $srcdir/support.ipv6.dns.patch
  find . -type f -exec sed -i 's@#!/usr.*python@#!/usr/bin/python2@' {} \;
  export PYTHON=python2
  python2 setup.py configure --no-install-init \
                             --no-install-gtk \
	                     --resume=/usr/share/wicd/scripts/ \
                             --suspend=/usr/share/wicd/scripts/ \
                             --verbose \
                             --python=/usr/bin/python2
}

package() {
  pkgdesc="Wired and wireless network manager for Linux"
  depends=('python2' 'dbus-python' 'dhcpcd' 'wpa_supplicant' 'wireless_tools'
           'inetutils' 'net-tools' 'ethtool' 'shared-mime-info' 'python2-urwid' 'pygobject')
  optdepends=('python-wpactrl:	needed if you want to use the new experimental ioctrl backend'
      	    'python-iwscan:	needed if you want to use the new experimental ioctrl backend'
            'wicd-gtk: needed if you want the GTK interface')
  backup=('etc/wicd/encryption/templates/active')
  install=wicd.install  
  cd $srcdir/$_pkgname-$pkgver
  # Dirty workaround, pybabel does not support Asturian language - https://bugs.launchpad.net/wicd/+bug/928589
  msgfmt po/ast.po -o translations/ast/LC_MESSAGES/wicd.mo

  python2 setup.py install --optimize=1 --root=$pkgdir
    # Add custom rc.d script
  install -Dm755 $srcdir/wicd-daemon $pkgdir/etc/rc.d/wicd
  cd build/lib/wicd
  for i in *.py; do
    install -Dm 755 $i $pkgdir/usr/lib/wicd/$i
  done
  rm -rf $pkgdir/usr/share/autostart

  #deleting the GTK stuff
  #rm -rf $pkgdir/etc/xdg
  #rm -f $pkgdir/usr/bin/{wicd-client,wicd-gtk}
  #rm -rf $pkgdir/usr/share/{applications,icons,pixmaps}
  #rm -rf $pkgdir/usr/share/wicd/gtk  
  #deleting systemd service files
  rm -rf $pkgdir/lib/systemd
}

