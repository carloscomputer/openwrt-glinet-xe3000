<!-- xe3000-preface:begin -->
![OpenWrt logo](include/logo.png)

# GL.iNet GL-XE3000 (Puli AX) — OpenWrt 25.12 with a working 5G modem

This branch (`xe3000`) builds a vanilla **OpenWrt 25.12** image for the
**GL.iNet GL-XE3000 (Puli AX)** with its **Quectel RM520N-GL 5G modem**, which
ModemManager drives over USB (QMI, `qmi_wwan`) on the unit this was checked on. Kernel: **6.12.118**.
(On the GL-X3000 vjt drives the same modem over PCIe/MHI; his patches for that path are in the
tree but do not apply to the XE3000.)

**This is not original work.** It is a 1:1 copy of
[vjt/openwrt-glinet-x3000](https://github.com/vjt/openwrt-glinet-x3000)
(tag `jeeves-r14`, already based on OpenWrt v25.12.6 plus newer branch commits), switched to the XE3000 device profile. The patch stack,
the build kit and the 5G tools are his. What differs is listed in
[`x3000/XE3000.md`](x3000/XE3000.md).

## Build it yourself

```
git clone --single-branch --branch xe3000 https://github.com/carloscomputer/openwrt-glinet-xe3000.git
cd openwrt-glinet-xe3000
./x3000/build.sh public -- -j1 V=s
```

The build prerequisites (packages, a Go toolchain for the bootstrap) are listed under
"Prerequisites" in [`x3000/README.md`](x3000/README.md). The image ends up in
`bin-x3000-public/openwrt-mediatek-filogic-glinet_gl-xe3000-squashfs-sysupgrade.bin`. A first build takes
hours; `-j1` keeps it on one CPU, which also keeps errors readable.
The signed apk feed (kernel modules and our packages) is published to the separate orphan branch `gh-pages` of this repository; `--single-branch` keeps those binaries out of your clone.
**Why is the directory still called `x3000/`?** It is vjt's build kit, and it builds
the XE3000 here. Keeping its name keeps his scripts, tests and documentation valid and
lets his updates be applied as a plain rebase. `x3000/README.md` is his text for the
GL-X3000; a note at its top explains what changes for the XE3000.

## On top of vjt's image

Packages added to both the public and the private image: `openvpn-openssl`, `kmod-ovpn-backports`, `luci-app-openvpn`, `kmod-wireguard`, `wireguard-tools`, `luci-proto-wireguard`, `tailscale`.
Own package sources can be added through `x3000/xe3000-packages.txt` (see
[`x3000/XE3000.md`](x3000/XE3000.md)); vjt's own mechanism for private overlays is
described under "Baking your own packages" in `x3000/README.md`.

## Flashing

Use the `...-squashfs-sysupgrade.bin`. vjt reports that the stock GL.iNet U-Boot
rejects the factory image on the X3000 and that sysupgrade is the way in; this fork
assumes the same for the XE3000 and has not verified another route. Keep a copy of
your original firmware and configuration, and note that flashing third-party firmware
may void your warranty.

## Pre-built images

If there are any, they are on the
[releases page](https://github.com/carloscomputer/openwrt-glinet-xe3000/releases) of this repository.

## Credits and licence

OpenWrt is licensed under GPL-2.0 (see below); the GL-X3000 work and the 5G tooling are
[vjt](https://github.com/vjt)'s. This repository is not affiliated with GL.iNet or Quectel.

<!-- xe3000-preface:end -->
The rest of this README is upstream OpenWrt's, kept verbatim for
reference.

---

OpenWrt Project is a Linux operating system targeting embedded devices. Instead
of trying to create a single, static firmware, OpenWrt provides a fully
writable filesystem with package management. This frees you from the
application selection and configuration provided by the vendor and allows you
to customize the device through the use of packages to suit any application.
For developers, OpenWrt is the framework to build an application without having
to build a complete firmware around it; for users this means the ability for
full customization, to use the device in ways never envisioned.

Sunshine!

## Download

Built firmware images are available for many architectures and come with a
package selection to be used as WiFi home router. To quickly find a factory
image usable to migrate from a vendor stock firmware to OpenWrt, try the
*Firmware Selector*.

* [OpenWrt Firmware Selector](https://firmware-selector.openwrt.org/)

If your device is supported, please follow the **Info** link to see install
instructions or consult the support resources listed below.

## 

An advanced user may require additional or specific package. (Toolchain, SDK, ...) For everything else than simple firmware download, try the wiki download page:

* [OpenWrt Wiki Download](https://openwrt.org/downloads)

## Development

To build your own firmware you need a GNU/Linux, BSD or macOS system (case
sensitive filesystem required). Cygwin is unsupported because of the lack of a
case sensitive file system.

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.

```
binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev
make4.1+ perl python3.7+ rsync subversion unzip which
```

### Quickstart

1. Run `./scripts/feeds update -a` to obtain all the latest package definitions
   defined in feeds.conf / feeds.conf.default

2. Run `./scripts/feeds install -a` to install symlinks for all obtained
   packages into package/feeds/

3. Run `make menuconfig` to select your preferred configuration for the
   toolchain, target system & firmware packages.

4. Run `make` to build your firmware. This will download all sources, build the
   cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen
   applications for your target system.

### Related Repositories

The main repository uses multiple sub-repositories to manage packages of
different categories. All packages are installed via the OpenWrt package
manager called `opkg`. If you're looking to develop the web interface or port
packages to OpenWrt, please find the fitting repository below.

* [LuCI Web Interface](https://github.com/openwrt/luci): Modern and modular
  interface to control the device via a web browser.

* [OpenWrt Packages](https://github.com/openwrt/packages): Community repository
  of ported packages.

* [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically
  focused on (mesh) routing.

* [OpenWrt Video](https://github.com/openwrt/video): Packages specifically
  focused on display servers and clients (Xorg and Wayland).

## Support Information

For a list of supported devices see the [OpenWrt Hardware Database](https://openwrt.org/supported_devices)

### Documentation

* [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
* [User Guide](https://openwrt.org/docs/guide-user/start)
* [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
* [Technical Reference](https://openwrt.org/docs/techref/start)

### Support Community

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advise.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## License

OpenWrt is licensed under GPL-2.0
