# Maintainer: Mads Mogensen <mads256h at gmail dot com>

pkgname=centralhub-git
pkgver=c21ed62
pkgrel=1
pkgdesc="Central hub stuff"
arch=('x86_64' 'armv7h')
url=https://github.com/cs-23-sw-7-06/CentralHub
license=('GPL3')
depends=('dotnet-sdk' 'aspnet-runtime')
makedepends=('git')
source=("$pkgname::git+https://github.com/cs-23-sw-7-06/CentralHub.git" "centralhub-api.service" "centralhub-webui.service")
sha256sums=('SKIP'
            'a3b87d395f8ca30aa684a24a2a6f6557a815eab4a175fd75c444124687cad021'
            'a1dae7b241594e73c22009a05ab545472e461a38ec235ce485850981fca9618e')

if [[ $CARCH == 'x86_64' ]]; then
  _arch='x64'
elif [[ $CARCH == 'armv7h' ]]; then
  _arch='arm'
fi

pkgver() {
  cd "$srcdir/$pkgname/"
  git rev-parse --short HEAD
}

build() {
  cd "$srcdir/$pkgname/"

  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
  export DOTNET_NOLOGO=1

  dotnet restore

# Build CentralHub.Api
  dotnet publish CentralHub.Api -c "Release" -o "${srcdir}/api/" --self-contained false --runtime "linux-$_arch" -p:DebugSymbols=false -p:DebugType=none

# Build CentralHub.WebUI
  dotnet publish CentralHub.WebUI -c "Release" -o "${srcdir}/webui/" --self-contained false --runtime "linux-$_arch" -p:DebugSymbols=false -p:DebugType=none
}

check() {
  cd "$srcdir/$pkgname"
  dotnet test
}

package () {
  mkdir -p "${pkgdir}/usr/lib/"
  cp -dr --no-preserve='ownership' "${srcdir}/api" "${pkgdir}/usr/lib/centralhub-api"
  cp -dr --no-preserve='ownership' "${srcdir}/webui" "${pkgdir}/usr/lib/centralhub-webui"

# Make programs available in path
  mkdir -p "${pkgdir}/usr/bin/"
  ln -s "/usr/lib/centralhub-api/CentralHub.Api" "${pkgdir}/usr/bin/centralhub-api"
  ln -s "/usr/lib/centralhub-webui/CentralHub.WebUI" "${pkgdir}/usr/bin/centralhub-webui"

# Systemd services
  install -D -m644 "${srcdir}/centralhub-api.service" "${pkgdir}/usr/lib/systemd/system/centralhub-api.service"
  install -D -m644 "${srcdir}/centralhub-webui.service" "${pkgdir}/usr/lib/systemd/system/centralhub-webui.service"
}

