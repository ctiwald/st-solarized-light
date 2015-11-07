pkgname=st-solarized-light
_pkgname=st
pkgver=20151106.00873e6
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='http://git.suckless.org/st/'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext')
source=('git://git.suckless.org/st' 'config.h' 'solarized.diff')
sha1sums=('SKIP' '16530cca6f23cb600de4fb1e00b78c3d632594d4' '827ec69aa0166d70cb3cdb3313f56f179f9d0beb')

provides=("${_pkgname}")
conflicts=("${_pkgname}")

pkgver() {
	cd "${srcdir}/${_pkgname}"
	git log -1 --format='%cd.%h' --date=short | tr -d -
}

prepare() {
	local file
	cd "${srcdir}/${_pkgname}"
	sed '/@tic/d' -i Makefile
	for file in "${source[@]}"; do
		if [[ "$file" == "config.h" ]]; then
			# add config.h if present in source array
			# Note: this supersedes the above sed to config.def.h
			cp "$srcdir/$file" .
			continue
		elif [[ "$file" == *.diff ]]; then
			# add all patches present in source array
			patch -Np1 <"$srcdir/$file"
		fi
	done
}

build() {
	cd "${srcdir}/${_pkgname}"
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
	cd "${srcdir}/${_pkgname}"
	make PREFIX=/usr DESTDIR="${pkgdir}" install
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
