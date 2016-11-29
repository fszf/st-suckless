# Maintainer: mar77i <mar77i at mar77i dot ch>
# Past Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>
# Modified for personal use: frank604


pkgname=st-fs
_pkgname=st
pkgver=0.7.16.ge448324
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='http://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
options=('zipman')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
epoch=1
# include config.h and any patches you want to have applied here
source=('git://git.suckless.org/st'
        '1-scrollback-0.7.diff'
        '2-personal.diff'
        '3-copypaste.diff'
        '4-color-base16-grayscale-dark.diff'
        #'4-color-gruvbox.diff'
        #'4-color-jasonwryan-dark.diff'
        '5-scrollback-mouse.diff'
        '6-scrollback-mouse-altscreen.diff'
        '7-clipboard.diff'
        '8-vertcentre.diff'
        '9-delkey.diff'
        '10-externalpipe.diff'
        '11-extpipe.diff'
        )
sha1sums=('SKIP'
          'c4e82b491bba7a78777647ff16624e3ffb570937'
          '5bd73c55e57fdd0485887a3cc7a41a5439a06d58'
          '463b05d46c5e1206c119087e7b2a937e8d2f2992'
          '53f55c3e4adc91759202dc6c93cc2d741ce83e8c'
          '88b85b0f3dff3606c5c791ab5752fdfb36727c7c'
          'a891faa40d51641dc3f54d472cdcfa8fa83e6fc7'
          'c916dc2410a0e43a06ace5c23a199ece273409c8'
          'dcde03cab595bdacc51011bcfc5d520611b9a03f'
          '7521f301fe525aa6eb2c2579d5212260d94bbe52'
          'fa02e1690f53884fac3a0628b57bb6de0a607564'
          '706ebf978e8479e3061c1e856c5df3158d9fa1d5')

provides=("${_pkgname}")
conflicts=("${_pkgname}")

pkgver() {
	cd "${_pkgname}"
	git describe --tags |sed 's/-/./g'
}

prepare() {
	local file
	cd "${_pkgname}"
	sed \
		-e '/char font/s/= .*/= "Fixed:pixelsize=13:style=SemiCondensed";/' \
		-e '/char worddelimiters/s/= .*/= " '"'"'`\\\"()[]{}<>|";/' \
		-e '/int defaultcs/s/= .*/= 1;/' \
		-i config.def.h
	sed \
		-e 's/CPPFLAGS =/CPPFLAGS +=/g' \
		-e 's/CFLAGS =/CFLAGS +=/g' \
		-e 's/LDFLAGS =/LDFLAGS +=/g' \
		-e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
		-i config.mk
	sed '/@tic/d' -i Makefile
	for file in "${source[@]}"; do
		if [[ "$file" == "config.h" ]]; then
			# add config.h if present in source array
			# Note: this supersedes the above sed to config.def.h
			cp "$srcdir/$file" .
		elif [[ "$file" == *.diff || "$file" == *.patch ]]; then
			# add all patches present in source array
			patch -Np1 <"$srcdir/$(basename ${file})"
		fi
	done
}

build() {
	cd "${_pkgname}"
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
	cd "${_pkgname}"
	make PREFIX=/usr DESTDIR="${pkgdir}" install
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
