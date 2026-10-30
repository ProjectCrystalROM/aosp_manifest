# Project Crystal AOSP5.1.1 / MS330

Dedicated AOSP foundation for the LG K7/MS330. Unchanged projects use official
android-5.1.1_r38; 28 adapted/imported providers use ProjectCrystalROM aosp_* repos.
CM12.1 repositories and branches are independent and are not modified here.

## Source checkout

```
mkdir -p LG_K7/aosp511
cd LG_K7/aosp511
repo init -u https://github.com/ProjectCrystalROM/aosp_manifest -b aosp-5.1.1-ms330
repo sync -j4
```

Use `repo init ... -m m1-20261030.xml` for the pinned publication checkpoint.
The default manifest tracks the dedicated branches. Do not sync over an existing
checkout with uncommitted bringup work. Pinned subproject SHAs are in the manifest;
unchanged AOSP projects retain the official release tag.

## Build

Read device/lge/m1/AOSP_RELEASE.md and device/lge/m1/webview/README.md. The ROM uses
Trusty14.04, Java7 and the AOSP ARM GCC4.8 toolchain; M95 builds on Debian through
GN/Ninja and the fixed-purpose host worker. This is currently a documented local
WSL/chroot layout, not a portable build container.

Supply matching chroot mounts/user, 32bit host libc/GCC runtime and pinned Chromium
source/tools. Stage ignored microG/CrystalGram release inputs as documented; their
hashes/signers are pinned in vendor source. No private API/Firebase/signing inputs
are needed for CrystalGram preinstallation; use its published release APK.

```
bash device/lge/m1/webview/sync-source.sh
bash device/lge/m1/webview/build-rom.sh
```

Run these scripts from the documented workspace layout (source folder aosp511).
Default profile is user-release: user variant, SELinux enforcing, authenticated
ADB, MTP default, no early diagnostic USB. Kernel3.10.108 and WebView95 are source
build dependencies. CrystalGram/microG are signed APK inputs; LG HALs/firmware and
stock archived DT remain binary inputs. TWRP is the only supported recovery and
is preserved by OTA packaging. No root integration, no Dialer/full SMS sender UI.

Build outputs initially use test keys. Supply private ROM release keys and perform
release signing/auditing before distribution. Never commit private keys or API
configuration. Original app signing identities must be preserved.

## Qualification

The owner validated M95, receive-only SMS and CrystalGram, plus Wi-Fi/Bluetooth/
LTE/reboot persistence/camera photo+video/audio input+output on intermediate builds.
User variant and SELinux enforcing then passed hardware checks. The final image's
diagnostic-USB-removal boot/authorization acceptance remains pending its build.
Boot logo clipping is a cosmetic known issue pending custom artwork. No new complete
ASB date is claimed: source patch label remains 2016-07-05; kernel is Linux3.10.108.

CrystalGram standalone releases: https://github.com/ProjectCrystalROM/CrystalGram/releases
