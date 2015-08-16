# Maintainer: ssf <punx69 at gmx dot net>
pkgname=kvantum-theme-glare-svn
pkgver=r453
pkgrel=1
pkgdesc="bright simple kvantum theme"
arch=("any")
url="http://sixsixfive.deviantart.com/art/Glare-494114516"
license=("CCPL:cc-by-sa-4.0")
depends=()
optdepends=('xfce-theme-glare-svn: for GTK+ support'
			'qt4-style-kvantum-svn: for Qt4 support'
			'qt5-style-kvantum-svn: for Qt5 support')
makedepends=('subversion' 'yad' 'sed' 'imagemagick' 'gawk' 'coreutils'
			'bc' 'findutils')
provides=("kvantum-theme-glare=${pkgver}")
conflicts=()
replaces=()
source=()
sha384sums=()
sha512sums=()

_svntrunk=https://github.com/sixsixfive/Glare/trunk/Glare-misc
_svnmod=Glare

pkgver() {
	svn log $_svntrunk --config-dir "$srcdir" | awk 'NR==2' | awk '{print $1}'
}

build() {
	cd "$srcdir"
	msg "Connecting to SVN server...."
	if [ -d "$_svnmod/.svn" ]; then
		(
		cd "$_svnmod"
		svn status --config-dir "$srcdir" --no-ignore | grep -E '(^\?)|(^\I)|(^\M)' | sed -e 's/^. *//' | sed -e 's/\(.*\)/"\1"/' | xargs rm -drf
		svn up .
		)
	else
		svn co "$_svntrunk" --config-dir "$srcdir" "$_svnmod"
	fi
	msg "SVN checkout done or server timeout"
}

package() {
	cd ${srcdir}/${_svnmod}
	printf "Would you like to change the highlight/selection color now?: [y/n] \n"
	read input
	if [ "$input" = "y" ]; then
		msg "executing color script."
		sleep 2
		./changecolor.sh
	else
		msg "skipped"
		sleep 2
	fi
	mkdir -p ${pkgdir}/usr/share/Kvantum
	cp -R Kvantum/Glare ${pkgdir}/usr/share/Kvantum/Glare
	mkdir -p ${pkgdir}/usr/share/apps/color-schemes
	cp Kvantum/Glare/configs/Glare.colors ${pkgdir}/usr/share/apps/color-schemes/Glare.colors
}
