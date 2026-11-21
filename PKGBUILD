# 简体中文版 Zed 的 loong64 原生构建配方
#
# 用法（在 LoongArch Arch Linux 上，于本仓库根目录）：
#   makepkg -f
#   sudo pacman -U zed-zhcn-<ver>-<rel>-loong64.pkg.tar.zst
#
# 与 script/linux 的区别：不下载源码，直接编译当前工作树；
# 词条资源位于 crates/zed_i18n/locales，改动后需重新编译 zed_i18n
# （crates/zed_i18n/build.rs 已声明 rerun-if-changed）。
pkgname=zed-zhcn
pkgver=1.31.0
pkgrel=1
pkgdesc='Zed editor with Simplified Chinese localization (ReachForStar/zed-ZH_CN), loong64 build'
arch=('loong64')
url='https://github.com/ReachForStar/zed-ZH_CN'
license=('GPL-3.0-or-later' 'AGPL-3.0-or-later' 'Apache-2.0')
depends=(alsa-lib curl fontconfig git glib2 glibc libgcc libstdc++ libxcb libx11
         libxkbcommon libxkbcommon-x11 netcat nodejs npm sqlite vulkan-driver
         vulkan-icd-loader vulkan-tools wayland zstd)
makedepends=(cargo clang cmake gettext git nodejs npm protobuf rust)
optdepends=('org.freedesktop.secrets: to keep you logged into your Zed account')
provides=('zed')
conflicts=('zed' 'zed-loc' 'zed-editor')
replaces=('zed-editor')

_binname=zeditor
_appid=dev.zed.Zed

source=()
sha256sums=()

build() {
	cd "$startdir"
	export RELEASE_VERSION="$pkgver"
	export ZED_UPDATE_EXPLANATION='Updates are handled by pacman'
	export PROTOC=/usr/bin/protoc
	export DO_STARTUP_NOTIFY=true
	# 低内存机器可按需调低并行度，例如 -j4
	cargo build --release --locked -j"$(nproc)" -p zed -p cli
}

package() {
	cd "$startdir"

	install -Dm0755 target/release/cli "$pkgdir/usr/bin/$_binname"
	install -Dm0755 target/release/zed "$pkgdir/usr/lib/zed/zed-editor"

	export APP_ICON=zed APP_NAME=Zed APP_CLI="$_binname" APP_ID="$_appid" APP_ARGS='%U'
	export BRANDING_LIGHT='#99c1f1' BRANDING_DARK='#1a5fb4'
	envsubst < crates/zed/resources/zed.desktop.in > "$_appid.desktop"
	install -Dm0644 "$_appid.desktop" "$pkgdir/usr/share/applications/$_appid.desktop"
	envsubst < crates/zed/resources/flatpak/zed.metainfo.xml.in \
		| sed '/@release_info@/d' > "$_appid.metainfo.xml"
	install -Dm0644 "$_appid.metainfo.xml" "$pkgdir/usr/share/metainfo/$_appid.metainfo.xml"
	install -Dm0644 crates/zed/resources/app-icon.png \
		"$pkgdir/usr/share/icons/hicolor/512x512/apps/zed.png"

	local cli="$startdir/target/release/cli"
	"$cli" --zed "$startdir/target/release/zed" --completions bash \
		> "$_binname.bash"
	install -Dm0644 "$_binname.bash" \
		"$pkgdir/usr/share/bash-completion/completions/$_binname"
	"$cli" --zed "$startdir/target/release/zed" --completions elvish \
		> "$_binname.elv"
	install -Dm0644 "$_binname.elv" "$pkgdir/usr/share/elvish/lib/$_binname.elv"
	"$cli" --zed "$startdir/target/release/zed" --completions fish \
		> "$_binname.fish"
	install -Dm0644 "$_binname.fish" \
		"$pkgdir/usr/share/fish/vendor_completions.d/$_binname.fish"
	"$cli" --zed "$startdir/target/release/zed" --completions nushell \
		> "$_binname.nu"
	install -Dm0644 "$_binname.nu" \
		"$pkgdir/usr/share/nushell/vendor/autoload/$_binname.nu"
	"$cli" --zed "$startdir/target/release/zed" --completions zsh \
		> "_$_binname"
	install -Dm0644 "_$_binname" "$pkgdir/usr/share/zsh/site-functions/_$_binname"
}
