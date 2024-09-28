# Maintainer: Luke Featherston <lukefeatherston1223 at gmail dot com>
# Contributor: Aldo Adirajasa Fathoni <aldo dot alfathoni at gmail dot com>
pkgname=gearlever
pkgver=2.0.7
pkgrel=1
pkgdesc="Manage AppImages with ease"
arch=('all')
url="https://mijorus.it/projects/gearlever/"
license=('GPL-3')
depends=( 'binutils' 'libfuse2' 'libgdk-pixbuf-2.0-0' 'libc6' 'libglib-2.0-0' 'gtk4-binver-4.0.0' 'hicolor-icon-theme' 'libadwaita-1-0' 'p7zip' 'libpango-1.0-0' 'python3' 'python3-dbus' 'python3-gi' 'python3-xdg' 'python3-requests' 'libz1' )
makedepends=( 'gettext' 'meson' 'python3-dbus-dev' 'python3-gi-dev' 'python3-xdg-dev' 'python3-requests-dev')
checkdepends=( 'appstream' 'desktop-file-utils' )
source=(
	"${pkgname}-${pkgver}.tar.gz"::"https://github.com/mijorus/gearlever/archive/refs/tags/${pkgver}.tar.gz"
)
sha256sums=(
	'772721843b6c323f7f4e74e4da3d72879f2b194d3d4ad981a8b783235fe270a1'
)

prepare() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	
	# Arch command doesn't seem to exist on arch linux so instead we can substitute it for 'uname' for now
	sed -i 's/\['\''arch'\''\]/\['\''uname'\'', '\''-m'\''\]/g' src/AppDetails.py
}

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	meson setup build --prefix=/usr
	meson compile -C build
}

check() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	meson test -C build --print-errorlogs
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	meson install -C build --destdir "${pkgdir}"
	
	cd "${pkgdir}"
	rm -v "usr/share/icons/hicolor/scalable/actions/meson.build"
	rm -v "usr/share/${pkgname}/${pkgname}/meson.build"
}
