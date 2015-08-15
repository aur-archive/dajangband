# Contributor: SaThaRiel <sathariel74[at]gmail[dot]com>
pkgname=dajangband
pkgver=1.3.5b
pkgrel=1
pkgdesc="Its purpose is to add a little flavor and character to Vanilla Angband. This is done mostly through redoing the monster list and adding a few new classes and races. Other changes are mainly to complement the changes in the monster list and new classes."
arch=('i686' 'x86_64')
url="http://sites.google.com/site/dajangbandwebsite/home"
license=('custom')
depends=('ncurses' 'libx11')
source=("DaJAngband135b.zip::https://sites.google.com/site/dajangbandwebsite/home/download-page/DaJAngband135b.zip?attredirects=0&d=1")
md5sums=('42b57095030e7894a857bb72587fc879')
 
build() {
  cd $srcdir/DaJAngband135/src
# Change the standard build parameters
  sed -e 's/SYS_gcu\ \=\ \-DUSE_GCU\ \-DUSE_NCURSES\ \-lncurses/SYS_gcu\ \=\ \-DUSE_GCU\ \-DUSE_CURSES\ \-lcurses/g' Makefile.std > Makefile.tmp
# Change the file of the executable
  sed -e 's/EXE\ \=\ angband/EXE\ \=\ dajangband/' Makefile.tmp > Makefile
# Fix the build via Makefile.src and Makefile.inc to run under Linux
  sed -e '12{s/z-form.h/z-form.h z-queue.h/};16{s/z-virt.o/z-virt.o z-queue.o/}' Makefile.src > Makefile.src2
  mv Makefile.src2 Makefile.src
  sed -e '22az-queue.o: z-queue.h' Makefile.inc > Makefile.inc2
  mv Makefile.inc2 Makefile.inc
# Fix the lib path to be in a good location
  mv config.h config.h.org
  sed -e 's/\#\ define\ DEFAULT_PATH\ \"\.\/lib\/\"/\#\ define\ DEFAULT_PATH\ \"\/usr\/lib\/dajangband\/\"/' config.h.org > config.h
# get rid of the comment in comment warnings from gcc
  mv defines.h defines.h.org
  sed -e '/^\/\*.*\/\*.*/d' defines.h.org > defines.h

# start the build and install
  make || return 1
  install -d "$pkgdir/usr/bin/" || return 1
  install -d "$pkgdir/usr/lib/$pkgname" || return 1
  cp -p ../$pkgname "$pkgdir/usr/bin/" || return 1
  cp -rp ../lib/* "$pkgdir/usr/lib/$pkgname/" || return 1
  chown -R root:games "$pkgdir/usr/lib/$pkgname/"
  chmod 775 "$pkgdir/usr/lib/$pkgname/apex"
  chmod 775 "$pkgdir/usr/lib/$pkgname/save"
  chmod 775 "$pkgdir/usr/lib/$pkgname/data"
  find $pkgdir/usr/lib/$pkgname/ -name delete.me -exec rm {} \;
  find $pkgdir/usr/lib/$pkgname/ -name 'Makefile*' -exec rm {} \;
}
