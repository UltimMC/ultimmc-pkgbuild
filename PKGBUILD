# Maintainer: Local Build
pkgname=ultimmc-git
pkgver=r3169.c95dfe89
pkgrel=1
pkgdesc="Free, open source launcher and instance manager for Minecraft."
arch=('x86_64')
url="https://github.com/UltimMC/Launcher"
license=('Apache-2.0')
depends=('zlib' 'libgl' 'qt5-base' 'qt5-x11extras' 'qt5-svg' 'xorg-xrandr' 'zenity' 'wget' 'unzip' 'java-runtime')
makedepends=('git' 'cmake' 'jdk17-openjdk')
provides=('ultimmc' 'ultimmc-bin')
conflicts=('ultimmc' 'ultimmc-bin')
source=("Launcher::git+https://github.com/UltimMC/Launcher.git"
        "https://raw.githubusercontent.com/UltimMC/ultimmc-deb/master/ultimmc/opt/ultimmc/icon.svg"
        "https://raw.githubusercontent.com/UltimMC/ultimmc-deb/master/ultimmc/usr/share/man/man1/ultimmc.1")
sha256sums=('SKIP'
            'SKIP'
            'ef4957acc5be202049b46039512418df6556e409b816967b4949940bfadc2933')

pkgver() {
    cd "$srcdir/Launcher"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "$srcdir/Launcher"
    git -c protocol.file.allow=always submodule update --init --recursive
    find . -name "CMakeLists.txt" -exec sed -i -E 's/(-source|-target)[[:space:]]+(1\.)?7/\1 8/g; s/"(-source|-target)"[[:space:]]+"(1\.)?7"/"\1" "8"/g' {} +
}

build() {
    export JAVA_HOME="/usr/lib/jvm/java-17-openjdk"
    export PATH="$JAVA_HOME/bin:$PATH"

    rm -rf "$srcdir/build"
    cmake -B "$srcdir/build" -S "$srcdir/Launcher" \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="/opt/ultimmc" \
        -DCMAKE_POLICY_VERSION_MINIMUM=3.5 \
        -DLauncher_LAYOUT=lin-nodeps \
        -DLauncher_EMBED_SECRETS=ON

    cmake --build "$srcdir/build" -j"$(nproc)"
}

package() {
    DESTDIR="$pkgdir" cmake --install "$srcdir/build"

    install -Dm644 "$srcdir/icon.svg" "$pkgdir/opt/ultimmc/icon.svg"
    install -Dm644 "$srcdir/icon.svg" "$pkgdir/usr/share/pixmaps/ultimmc.svg"
    install -Dm644 "$srcdir/ultimmc.1" "$pkgdir/usr/share/man/man1/ultimmc.1"

    install -dm755 "$pkgdir/usr/bin"
    cat << 'RUNEOF' > "$pkgdir/opt/ultimmc/run.sh"
#!/bin/sh
mkdir -p "$HOME/.local/share/ultimmc"
if [ -x /opt/ultimmc/bin/UltimMC ]; then
    exec /opt/ultimmc/bin/UltimMC -d "$HOME/.local/share/ultimmc" "$@"
else
    exec /opt/ultimmc/UltimMC -d "$HOME/.local/share/ultimmc" "$@"
fi
RUNEOF
    chmod 755 "$pkgdir/opt/ultimmc/run.sh"
    ln -s "/opt/ultimmc/run.sh" "$pkgdir/usr/bin/ultimmc"

    install -dm755 "$pkgdir/usr/share/applications"
    cat << 'DESKEOF' > "$pkgdir/usr/share/applications/ultimmc.desktop"
[Desktop Entry]
Name=UltimMC
Comment=Free, open source launcher and instance manager for Minecraft
Exec=/usr/bin/ultimmc
Icon=ultimmc
Terminal=false
Type=Application
Categories=Game;
DESKEOF
}
