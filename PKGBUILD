# P7Zip-GUI: Installer: Arch
# Contributor: Xavion <Xavion (dot) 0 (at) Gmail (dot) com>

realname=p7zip
pkgname=${realname}-gui
pkgver=9.13
pkgrel=2
pkgdesc="The GUI component of the P7Zip compression utility"
arch=("i686" "x86_64")
license=("GPL")
url="http://${realname}.sourceforge.net"
depends=("wxgtk" "${realname}=${pkgver}")
makedepends=("make")
optdepends=("q7z: A P7Zip GUI, which attempts to simplify data compression and backup")
options=(!emptydirs)
source=(http://downloads.sourceforge.net/sourceforge/${realname}/${realname}_${pkgver}_src_all.tar.bz2)

build() {
	cd ${srcdir}/${realname}_${pkgver}

	#Arch64 fix
	if [ "${CARCH}" == "x86_64" ]; then
		cp makefile.linux_amd64 makefile.machine
	else
		cp makefile.linux_x86_ppc_alpha_gcc_4.X makefile.machine
	fi

	# Build
	make 7zG 7zFM OPTFLAGS="${CXXFLAGS}"
}

package() {
	cd ${srcdir}/${realname}_${pkgver}
	
	# The "make install" script does not install the GUI. Install it
	# according to GUI/readme.txt.
	local share="${pkgdir}/usr/lib/${realname}"
	
	mkdir -p "${pkgdir}/usr/bin"
	local name
	for name in 7zG 7zFM; do
		install -m755 -D "bin/${name}" "${share}/${name}"
		
		# Wrapper scripts needed so that the program knows the real
		# directory it's installed in. Symbolic links don't work.
		cat << SH > "${pkgdir}/usr/bin/${name}"
#! /bin/sh
/usr/lib/${realname}/${name} "\$@"
SH
		chmod 755 "${pkgdir}/usr/bin/${name}"
	done
	
	cp -r GUI/help "${pkgdir}/usr/lib/${realname}/"
	find "${share}/help" -type d -exec chmod 755 {} ';'
	find "${share}/help" -type f -exec chmod 644 {} ';'
	
	install -m755 -D GUI/${realname}ForFilemanager ${pkgdir}/usr/bin/${realname}ForFilemanager
	install -m755 -D GUI/${realname}_16_ok.png ${pkgdir}/usr/share/icons/hicolor/16x16/apps/${realname}.png
	ln -s 7zCon.sfx ${pkgdir}/usr/lib/${realname}/7z.sfx
}

sha1sums=('81da0729561ce123c0a82656ec96a04ad5bfa522')
