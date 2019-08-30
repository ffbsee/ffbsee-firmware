Firmware for Freifunk Bodensee BATMAN_V network
=========================

The firmware turns a common wireless router into a mesh networking device.
It connects to similar routers in the area and builds a Wifi-mesh network
but also opens an access point for computers to connect over Wifi.
Included is Internet connectivity and a web interface.

Please talk to us on [IRC](https://webirc.hackint.org/#irc://irc.hackint.org/#ffbsee) if anything does not work!

[Precompiled firmware images](https://firmware.ffbsee.de//firmware/ "Precompiled firmware images") are available on our server. You can search them via [Firmware-Wizard](https://firmware.ffbsee.de/firmware-wizard/). All other released versions here on github are **out-of-date**.

To build the firmware yourself you need a Unix console to enter commands into.
Please have a look at the different Branches if you want to build the newest Beta-Firmware Version.

Install dependencies for the build environment (Debian/Ubuntu):

```bash
    sudo apt-get update; sudo apt-get upgrade
    sudo apt-get install subversion g++ zlib1g-dev build-essential git python
    sudo apt-get install libncurses5-dev gawk gettext unzip file libssl-dev wget yui-compressor
```
Build commands for the console:

```bash
git clone git://git.openwrt.org/source.git
cd source
git reset --hard 8bf8de95a20d9fee54bbd579832c578c62cbc901

git clone https://github.com/ffbsee/ffbsee-firmware.git
./minify-webstuff.sh
cp -rf ffbsee-firmware/files ffbsee-firmware/package ffbsee-firmware/feeds.conf .

./scripts/feeds update -a
./scripts/feeds install -a
git am --whitespace=nowarn ffbsee-firmware/patches/openwrt/*.patch

cd feeds/routing
git am --whitespace=nowarn ../../ffbsee-firmware/patches/routing/*.patch
cd ../../

rm -rf ffbsee-firmware tmp

make menuconfig
```
Now select the right "Target System" and "Target Profile" for your AP model:

For example, for the TL-WR841ND v3, select:
* `Target System => Atheros AR7xxx/AR9xxx`
* `Target Profile => <*> TP-LINK TL-WR842N/ND v3`

Or in case you have the Ubiquiti UniFi Outdoor, select:
* `Target System => Atheros AR7xxx/AR9xxx`
* `Target Profile => <*> Ubiquiti UniFi Outdoor`

For other models you can lookup the "Target System" in the OpenWrt
[hardware table](http://wiki.openwrt.org/toh/start). Your AP model
should now be visible in the "Target Profile" list.

Please notice, that some Routers need different drivers for 5GHz. Sometimes you need to select them manually.

Now start the build process. This takes some time:

```bash
    make
```
*You have the opportunity to compile the firmware at more CPU threats to speed up the process.*
*e.g. to run 3 jobs (commands) simultaneously use the following option:*
```bash
make -j 3
```

The **firmware image files** will be stored in the `bin`-folder. These images can now directly be used to update your router. Please note, that two differnt image types (per router) will be provided:

* Use `openwrt-[chip]-[model]-squashfs-factory.bin` for the initial flash (for routers running stock/vendor firmware).
* Use `openwrt-[chip]-[model]-squashfs-sysupgrade.bin` for futher updates (for routers having already another Freifunk FW flashed).

**Many routers have not been tested yet, but may work.**
***Give it a try! :-) ...and tell us about your experiences***

To build all images for all supported models see [github.com/freifunk-bielefeld](https://github.com/freifunk-bielefeld/docs/blob/master/release_howto.md#images-bauen)
