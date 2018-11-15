# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm64/configs/(CHANGEME!)

pkgname="linux-oneplus-oneplus3"
pkgver=3.18.71
pkgrel=0
pkgdesc="OnePlus 3/3T kernel fork"
arch="aarch64"
_carch="arm64"
_flavor="oneplus-oneplus3"
url="https://github.com/LineageOS/android_kernel_oneplus_msm8996"
license="GPL-2.0-only"
options="!strip !check !tracedeps"
makedepends="perl sed installkernel bash gmp-dev bc linux-headers elfutils-dev devicepkg-dev"

# Compiler: latest GCC from Alpine
HOSTCC="${CC:-gcc}"
HOSTCC="${HOSTCC#${CROSS_COMPILE}}"

# Source
_repository="android_kernel_oneplus_msm8996"
_commit="d28b28b53f3c453eb6a9bcb7f20787f3dcdfc466"
_config="config-${_flavor}.${arch}"
source="
	$pkgname-$_commit.tar.gz::https://github.com/LineageOS/${_repository}/archive/${_commit}.tar.gz
	$_config
"
builddir="$srcdir/${_repository}-${_commit}"

prepare() {
	default_prepare
	downstreamkernel_prepare "$srcdir" "$builddir" "$_config" "$_carch" "$HOSTCC"

}

build() {
	unset LDFLAGS
	make ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	# kernel.release
	install -D "$builddir/include/config/kernel.release" \
		"$pkgdir/usr/share/kernel/$_flavor/kernel.release"

	# zImage (find the right one)
	cd "$builddir/arch/$_carch/boot"
	_target="$pkgdir/boot/vmlinuz-$_flavor"
	for _zimg in zImage-dtb Image.gz-dtb *zImage Image; do
		[ -e "$_zimg" ] || continue
		msg "zImage found: $_zimg"
		install -Dm644 "$_zimg" "$_target"
		break
	done
	if ! [ -e "$_target" ]; then
		error "Could not find zImage in $PWD!"
		return 1
	fi
}

sha512sums="e43729b4366c6e6edb082f9e028fbcc0549d93c0873139b7da1c5153bdb081b0db573b6655c5635f6ad7fb1cf874066b07f217f767971f05c1395bd5f964d3f1  linux-oneplus-oneplus3-d28b28b53f3c453eb6a9bcb7f20787f3dcdfc466.tar.gz
591717504ac55bb6eac29ddb3dc156ca7232cd725d5d3b0dd3bee4815313c148982792b363fcee78561a5622301cbafe227f318a36a8b57af7bb98d013e3c060  config-oneplus-oneplus3.aarch64"