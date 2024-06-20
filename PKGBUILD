# Maintainer: Luke Featherston <lukefeatherston1223 at gmail dot com>
pkgname=gearlever
pkgver=1.6.3
pkgrel=1
pkgdesc="Manage AppImages with ease"
arch=('x86_64')
url="https://mijorus.it/projects/gearlever/"
license=('GPL-3.0-or-later')
depends=( 'dconf' 'fuse2' 'gdk-pixbuf2' 'glibc' 'glib2' 'gtk4' 'hicolor-icon-theme' 'libadwaita' 'p7zip' 'pango' 'python' 'python-dbus' 'python-gobject' 'python-pyxdg' 'zlib' )
makedepends=( 'gettext' 'meson' )
checkdepends=( 'appstream' 'desktop-file-utils' )
options=( '!strip' '!debug' )
source=(
	"${pkgname}-${pkgver}.tar.gz"::"https://github.com/mijorus/gearlever/archive/refs/tags/${pkgver}.tar.gz"
)
sha256sums=(
	'857a87994bd0d6e55c35ec5a185775dffa0fdab3b924332be4a964048c9915b0'
)

prepare() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	sed -i 's/cmd = \['\''flatpak-spawn'\'', '\''--host'\'', \*command\]/cmd = \[\*command\]/g' src/lib/terminal.py
}

build() {
	arch-meson "${srcdir}/${pkgname}-${pkgver}" build
	meson compile -C build
}

check() {
	# Only run validation tests on desktop and schema files as the appstream test fails at this time.
	# https://github.com/mijorus/gearlever/issues/76
	meson test "Validate desktop file" "Validate schema file" -C build --print-errorlogs
	# Instead, we can run an alternative to appstream_util with appstreamcli --no-net for validation.
	appstreamcli validate --no-net "${srcdir}/build/data/it.mijorus.gearlever.appdata.xml"
}

package() {
	meson install -C build --destdir "${pkgdir}"
	
	cd "${srcdir}/${pkgname}-${pkgver}"
	install -Dm644 COPYING -t "${pkgdir}/usr/share/licenses/$pkgname/"
	
	cd "${pkgdir}"
	rm -v "usr/share/icons/hicolor/scalable/actions/meson.build"
	rm -v "usr/share/${pkgname}/${pkgname}/meson.build"
}
