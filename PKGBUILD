# Maintainer: dreamscache.d <dreamscache.d@gmail.com>
_pkgname=recaf
pkgname=${_pkgname}-git
pkgver=r2278.e9765d4e
pkgrel=1
pkgdesc="A modern Java bytecode editor"
arch=("any")
url="https://github.com/Col-E/Recaf"
license=("MIT")
depends=("java-environment-openjdk=21" "ttf-font")
makedepends=("git" "jdk11-openjdk")
provides=("$_pkgname")
conflicts=("$_pkgname" "$_pkgname-bin")
replaces=("recaf")
source=("recaf::git+https://github.com/Col-E/Recaf#branch=master"
        "MavenUtil.patch"
        "settings.xml")
md5sums=("SKIP"
         "c6d14c04cb3313413e306a9e45cb6757"
         "4bde901d55aa06e6db09994266f19a3f")

pkgver() {
	cd "$srcdir/$_pkgname"
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "$srcdir/$_pkgname"
    patch "src/main/java/me/coley/recaf/util/MavenUtil.java" "$srcdir/MavenUtil.patch"
}

build() {
	cd "$srcdir/$_pkgname"
    JAVA_HOME="/usr/lib/jvm/java-11-openjdk" MAVEN_USER_HOME="$srcdir/$_pkgname/.m2" ./mvnw -gs "$srcdir/settings.xml" clean package
}

package() {
	cd "$srcdir/$_pkgname"
	install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/$_pkgname/LICENSE"
    install -Dm755 "target/$_pkgname-$(git describe --tags --abbrev=0)-J8-jar-with-dependencies.jar" "$pkgdir/usr/share/java/$_pkgname/$_pkgname.jar"
    install -Dm644 "src/main/resources/icons/logo.png" "$pkgdir/usr/share/pixmaps/recaf.png"
    printf '#!/usr/bin/env bash\nexec java -cp "/usr/lib/jvm/java-21-openjdk/lib/*:/usr/share/java/%s/%s.jar" "me.coley.recaf.Recaf" "$@"' "$_pkgname" "$_pkgname" > "recaf"
    printf "[Desktop Entry]\nType=Application\nVersion=1.0\nName=Recaf\nComment=%s\nPath=/usr/bin\nExec=sh -c 'if [ -n \"\$0\" ]; then recaf --input=\"\$0\"; else recaf; fi' %%u\nIcon=recaf\nTerminal=false\nMimeType=application/java-archive\nCategories=Development;Java" "$pkgdesc" > "recaf.desktop"
    install -Dm755 "recaf" "$pkgdir/usr/bin/recaf"
    install -Dm644 "recaf.desktop" "$pkgdir/usr/share/applications/recaf.desktop"
    chmod 775 "$pkgdir/usr/bin/$_pkgname"
}
