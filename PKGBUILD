# Maintainer: George Rawlinson <grawlinson@archlinux.org>

pkgname=python-rpds-py
pkgver=0.15.2
pkgrel=1
pkgdesc='Python bindings to the Rust rpds crate for persistent data structures'
arch=('x86_64')
url='https://github.com/crate-py/rpds'
license=('MIT')
depends=(
  'glibc'
  'gcc-libs'
  'python'
)
makedepends=(
  'git'
  'python-build'
  'python-maturin'
  'python-installer'
)
_commit='73b17e2ec1939a24e6996aaf65907808a15c217b'
source=("$pkgname::git+$url#commit=$_commit")
b2sums=('SKIP')

pkgver() {
  cd "$pkgname"

  git describe --tags | sed 's/^v//'
}

prepare() {
  cd "$pkgname"

  # download dependencies
  cargo fetch --locked --target "$CARCH-unknown-linux-gnu"
}

build() {
  cd "$pkgname"

  python -m build --wheel --no-isolation
}

package() {
  cd "$pkgname"

  python -m installer --destdir="$pkgdir" dist/*.whl

  # symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir/usr/share/licenses/$pkgname"
  ln -s "$site_packages/rpds_py-$pkgver.dist-info/license_files/LICENSE" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
