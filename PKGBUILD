# $Id: PKGBUILD 127845 2015-02-17 12:05:53Z fyan $
# Maintainer: Florian Pritz <flo@xinu.at>
# Contributor: Stéphane Gaudreault <stephane@archlinux.org>

_pkgbasename=krb5
pkgname=lib32-$_pkgbasename
pkgver=1.13.1
pkgrel=1
pkgdesc="The Kerberos network authentication system (32-bit)"
arch=('x86_64')
url="http://web.mit.edu/kerberos/"
license=('custom')
depends=('lib32-e2fsprogs' 'lib32-libldap' 'lib32-keyutils' "$_pkgbasename")
makedepends=('perl' 'gcc-multilib')
source=("http://web.mit.edu/kerberos/dist/${_pkgbasename}/1.13/${_pkgbasename}-${pkgver}-signed.tar"
        krb5-config_LDFLAGS.patch)
sha1sums=('2832695845d6c4cb0e7a622df4885f18acbd94cf'
          'f125824ed37f31e6fd2fdb6a437be8ff1c3700ab')
options=('!emptydirs')

prepare() {
   tar zxvf ${_pkgbasename}-${pkgver}.tar.gz
   cd "${srcdir}/${_pkgbasename}-${pkgver}"

   # cf https://bugs.gentoo.org/show_bug.cgi?id=448778
   patch -p1 -i "${srcdir}"/krb5-config_LDFLAGS.patch
}

build() {
   cd "${srcdir}/${_pkgbasename}-${pkgver}/src"

   export CC="gcc -m32"
   export CXX="g++ -m32"
   export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

   export CFLAGS+=" -fPIC -fno-strict-aliasing -fstack-protector-all"
   export CPPFLAGS+=" -I/usr/include/et"
   ./configure --prefix=/usr \
               --sysconfdir=/etc \
               --localstatedir=/var/lib \
               --libdir=/usr/lib32 \
               --enable-shared \
               --with-system-et \
               --with-system-ss \
               --disable-rpath \
               --without-tcl \
               --enable-dns-for-realm \
               --with-ldap \
               --without-system-verto

   make
}

#check() {
   # We can't do this in the build directory.

   # only works if the hostname is set properly/resolves to something. whatever...
   #cd "${srcdir}/${_pkgbasename}-${pkgver}"
   #make -C src check
#}

package() {
   cd "${srcdir}/${_pkgbasename}-${pkgver}/src"
   make DESTDIR="${pkgdir}" install

   rm -rf "${pkgdir}"/usr/{include,share,bin,sbin}
   mkdir -p "$pkgdir/usr/share/licenses"
   ln -s $_pkgbasename "$pkgdir/usr/share/licenses/$pkgname"
}
