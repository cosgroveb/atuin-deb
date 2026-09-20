# atuin-deb

Build private Debian packages from the official Atuin v18.20.0 musl binaries.
The build verifies pinned SHA256 checksums and installs the executable unchanged,
including upstream AI support.

```sh
sudo apt-get install ca-certificates curl dpkg-dev gzip
SUITE=sid ARCH=arm64 scripts/build-package
sudo apt install ./dist/atuin_18.20.0-1~sid_arm64.deb
atuin --version
```

`ARCH` defaults to the host architecture and accepts `arm64` or `amd64`. `SUITE`
defaults to the host codename (or `sid` when absent) and accepts `bookworm`,
`trixie`, `sid`, or `noble`. Override `PACKAGE_REVISION` for packaging changes.
The build needs no Rust toolchain and can package either architecture on either host.

The package installs `/usr/bin/atuin` and upstream documentation. Add
`eval "$(atuin init zsh)"` to your zsh configuration, `eval "$(atuin init bash)"`
for bash, or `atuin init fish | source` for fish.
AI features retain upstream authentication and service
requirements. Upgrade through APT rather than `atuin update`.

For a fresh user, enable the upstream daemon in `~/.config/atuin/config.toml`
so Atuin creates the encryption key needed to record history:

```toml
[daemon]
enabled = true
autostart = true
```

GitHub Actions builds both architectures for the supported suites and uploads
workflow artifacts. This repository builds private binary packages. It supplies
no Debian source package or package publishing service.

Version `18.20.0-1~sid` sorts below Debian's `18.20.0+ds-1`. Install the local
package explicitly and pin it in environments that must retain the upstream build.
