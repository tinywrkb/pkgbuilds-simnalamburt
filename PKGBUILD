# Maintainer: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

_nginxver=1.12.0
pkgname=nginx-mainline-mod-brotli
pkgver=$_nginxver
pkgrel=2

_modname="ngx_${pkgname#nginx-mainline-mod-}"

# https://github.com/google/ngx_brotli
# https://github.com/google/ngx_brotli/tree/master/deps
_modver=bfd2885b2da4d763fed18f49216bb935223cd34b
_brotliver=222564a95d9ab58865a096b8d9f7324ea5f2e03e

pkgdesc="Brotli compression filter module for mainline nginx"
arch=('i686' 'x86_64')
depends=('nginx-mainline')
url="https://github.com/google/ngx_brotli"
license=('CUSTOM')

source=(
	http://nginx.org/download/nginx-$_nginxver.tar.gz
	https://github.com/google/$_modname/archive/$_modver/$_modname-$_modver.tar.gz
	https://github.com/google/brotli/archive/$_brotliver/brotli-$_brotliver.tar.gz
)

sha256sums=('b4222e26fdb620a8d3c3a3a8b955e08b713672e1bc5198d1e4f462308a795b30'
            '3a5348d484554f3d1787d06961fc7886fda44d17264ab7e6cdf1f4a8fa04231e'
            '4299a2a86f0b931e80dd548be17fcaa5a6c158a0727f497f22cbb365668af0fe')

prepare() {
	cd "$srcdir"/$_modname-$_modver/deps
	rm -rf brotli
	ln -s ../../brotli-$_brotliver brotli
	export NGX_BROTLI_STATIC_MODULE_ONLY=1
}

build() {
	cd "$srcdir"/nginx-$_nginxver
	./configure $(nginx -V 2>&1 | grep 'configure arguments' | sed -r 's@^[^:]+: @@') --add-dynamic-module=../$_modname-$_modver
	make modules
}

package() {
	install -Dm644 "$srcdir"/$_modname-$_modver/LICENSE \
	               "$pkgdir"/usr/share/licenses/$pkgname/LICENSE

	cd "$srcdir"/nginx-$_nginxver/objs
	for mod in ngx_*.so; do
		install -Dm755 $mod "$pkgdir"/usr/lib/nginx/modules/$mod
	done
}
