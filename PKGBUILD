# Maintainer: Mads Mogensen <mads256h at gmail dot com>

pkgname=centralhub-git
pkgver=8b53787
pkgrel=1
pkgdesc="Central hub stuff"
arch=('x86_64' 'armv7h')
url=https://github.com/cs-23-sw-7-06/CentralHub
license=('gpl-3')
depends=('dotnet-sdk' 'aspnet-runtime')
source=("$pkgname::git+https://github.com/cs-23-sw-7-06/CentralHub.git" "centralhub-api.service" "centralhub-webui.service")
sha256sums=('SKIP'
            '8d4343d2f9223f86b9b27f413f79432f8270320e822e56e4534111f6cfcfd0b7'
            '038c33d924e6173fda32e99e2f8dde2d5c323ce04fd06a2fabaf90e4a3d856b5')

pkgver() {
	cd "$srcdir/$pkgname/"
	git rev-parse --short HEAD
}

build() {
  cd "$srcdir/$pkgname/"
  dotnet restore

# Build CentralHub.Api
  dotnet publish CentralHub.Api -c "Release" -o "${srcdir}/api/"

# Build CentralHub.WebUI
  dotnet publish CentralHub.WebUI -c "Release" -o "${srcdir}/webui/"
}

check() {
  cd "$srcdir/$pkgname"
  dotnet test
}

package () {
# Copy programs to /opt/centralhub/
  mkdir -p "${pkgdir}/opt/centralhub/"
  cp -r -a "${srcdir}/api/" "${pkgdir}/opt/centralhub/api"
  cp -r -a "${srcdir}/webui/" "${pkgdir}/opt/centralhub/webui"

# Make programs available in path
  mkdir -p "${pkgdir}/usr/bin/"
  ln -s "/opt/centralhub/api/CentralHub.Api" "${pkgdir}/usr/bin/centralhub-api"
  ln -s "/opt/centralhub/webui/CentralHub.WebUI" "${pkgdir}/usr/bin/centralhub-webui"

# Systemd services
  install -D -m644 "${srcdir}/centralhub-api.service" "${pkgdir}/usr/lib/systemd/system/centralhub-api.service"
  install -D -m644 "${srcdir}/centralhub-webui.service" "${pkgdir}/usr/lib/systemd/system/centralhub-webui.service"
}

