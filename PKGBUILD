# Maintainer: George Rawlinson <grawlinson@archlinux.org>

_os="$( \
  uname \
    -o)"
_py="python"
_pkg="rpds-py"
pkgname="${_py}-${_pkg}"
pkgver=0.19.0
pkgrel=1
pkgdesc='Python bindings to the Rust rpds crate for persistent data structures'
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'i686'
  'pentium4'
  'mips'
  'i686'
  'powerpc'
)
url='https://github.com/crate-py/rpds'
license=(
  'MIT'
)
depends=(
  "${_py}"
)
if [[ "${_os}" == "GNU/Linux" ]]; then
  depends+=(
    'glibc'
    'gcc-libs'
  )
elif [[ "${_os}" == "Android" ]]; then
  depends+=(
    'ndk-sysroot'
  )
fi
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-maturin"
  "${_py}-installer"
)
source=(
  "${pkgname}::git+${url}#tag=v${pkgver}"
)
sha512sums=(
  'a51c7d87acc1c63b69e62db80f387f36b4f2ad9b64efca1ca01a0b9998bd7dba4de5a245c5e2a5408e30817360d5b503ede243fbeac435896e6368134b38f7ea'
)
b2sums=(
  'bb6e76dcf064191742f1729c3177717c8da0a5ccee4039a92bbac86f08555d1a97f79df69d23d8fe23f4f4a6f4725b51f270e0ced13bd2b39770ddd9e7c16729'
)

prepare() {
  local \
    _target
  _target="${CARCH}-unknown-linux-gnu"
  [[ "${_os}" == "Android" ]] && \
    _target="${CARCH}-linux-androideabi"
  cd \
    "${pkgname}"
  # download dependencies
  CARGO_HTTP_MULTIPLEXING=false \
  cargo \
    fetch \
      --locked \
      --target \
      "${_target}"
}

build() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

package() {
  local \
    site_packages
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # symlink license file
  site_packages=$( \
    python \
      -c \
      "import site; print(site.getsitepackages()[0])")
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/rpds_py-${pkgver}.dist-info/license_files/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
