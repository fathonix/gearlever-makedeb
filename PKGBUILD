# Maintainer: Luke Featherston <lukefeatherston1223 at gmail dot com>
pkgname=gearlever
pkgver=1.5.2
pkgrel=1
pkgdesc="Manage AppImages with ease"
arch=('x86_64')
url="https://mijorus.it/projects/gearlever/"
license=('GPL-3.0-or-later')
depends=( 'dbus-python' 'dconf' 'fuse2' 'gdk-pixbuf2' 'glibc' 'glib2' 'gtk4' 'hicolor-icon-theme' 'libadwaita' 'p7zip' 'pango' 'python' 'python-gobject' 'python-pyxdg' 'zlib' )
makedepends=( 'meson' 'gettext' )
checkdepends=( 'desktop-file-utils' 'appstream' )
options=(!strip)
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/mijorus/gearlever/archive/refs/tags/1.5.2.tar.gz")
sha256sums=('f5c50f57de300917ea8d313563fab443c1a50206e5db5c3d1ccb5e81459acc82')

prepare() {
	# Change flatpak-spawn to instead execute the command natively since we are not inside a flatpak.
	cd "$srcdir/$pkgname-$pkgver"
	sed -i 's/cmd = \['\''flatpak-spawn'\'', '\''--host'\'', \*command\]/cmd = \[\*command\]/g' src/lib/terminal.py

}

build() {
	arch-meson "$srcdir/$pkgname-$pkgver" build
	meson compile -C build
}

check() {
	# Only run validation tests on desktop and schema files as the appstream test fails at this time.
	# https://github.com/mijorus/gearlever/issues/76
	meson test "Validate desktop file" "Validate schema file" -C build --print-errorlogs
	# Instead, we can run an alternative to appstream_util with appstreamcli --no-net for validation.
	appstreamcli validate --no-net "$srcdir/build/data/it.mijorus.gearlever.appdata.xml"
}

package() {
	meson install -C build --destdir "$pkgdir"
	cd "$srcdir/$pkgname-$pkgver"
	install -Dm644 COPYING -t "$pkgdir/usr/share/licenses/$pkgname/"
}
