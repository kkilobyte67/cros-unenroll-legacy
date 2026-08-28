# Unenrollment for Chrome OS Devices

## Table of Contents
- [Unenrollment](#unenrollment-for-chrome-os-devices)
  * [Why unenroll?](#why-unenroll-)
  * [What Kernver and ChromeOS version do I have?](#what-kernver-and-chromeos-version-do-i-have-)
- [Kernver to ChromeOS Table](#kernver--kv--to-chromeos--cros--table)
- [How do I unenroll?](#how-do-i-unenroll-)
  * [SHroot](#chromeos-v101-and-below---shroot)
  * [SH1mmer](#chromeos-v110-and-below---sh1mmer)
  * [CryptoSmite](#chromeos-v118-and-below---cryptosmite)
  * [BadRecovery](#chromeos-v124-and-below---badrecovery--formerly-olybmmer-------i-have-no-idea-how-accurate-this-guide-is-going-to-be-because-i-have-never-used-badrecovery-before----)
  * [icarus](##chromeos-v129-and-below---icarus)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Why unenroll?
Every method except the Shim method requires FWMP to be missing as Developer Mode is required, but FWMP blocks Developer Mode. Thus we need to remove FWMP. 

## What Kernver and ChromeOS version do I have?
To verify your kernver and your ChromeOS version, you need to first boot ChromeOS and then press `alt+v`, you will find a number followed by other numbers, only the first 2 or 3 numbers matter, such as `94`, `110` or `127`. Take note of this number and find the method related. If it is `120` or higher, you should check your kernver. Press `esc+⟳+⏻ ` (`esc+refresh+power`), and then `tab`. 

Now, you just scroll down with your eyes until you find `tpm_ver`, and then look to the right of that to find `tpm_kernver`, and your kernver will be the last number to the right of kernver, this means `tpm_kernver=0x00010004` is kernver 4 or kv4.

# Kernver (kv) to ChromeOS (crOS) table
Please note this can be inaccurate because kernver skipping (aka kernskip) is really common. For example, I own an `octopus-phaser360` Chromebook with kernver 1 but on ChromeOS v126 (v126 is kernver 4), and many people got ChromeOS v113 on kernver 1 (v113 is kernver 2).
In the event of a kernskip, you should downgrade to the versions connected to your kernver to be allowed access to more exploits.

| Kernver   | ChromeOS version            | Unenroll method |
|-----------|-----------------------------|---------------|
| 0<sup>1</sup> | any up to v111<sup>3</sup> | SHroot (to v101) or SH1mmer (to v110) |
| 1 | any up to v111<sup>3</sup> | SHroot (to v101) or SH1mmer (to v110) |
| 2<sup>2</sup> | v112 to v119<sup>4</sup>| Cryptosmite (to v118) |
| 3 | v120 to v125<sup>5</sup> | BadRecovery |
| 4 | v126 to v131 | icarus (to v129) |

<sub><sup>Kv0 is usually a factory setting bug<sup>1</sup></sup></sub> \
<sub><sup>On some devices, kv2 is actually crOS v111<sup>2</sup></sup></sub> \
<sub><sup>v110 or lower recommended<sup>3</sup></sup></sub> \
<sub><sup>v118 or lower recommended<sup>4</sup></sup></sub> \
<sub><sup>Some early versions of v120 is kv2, however newer versions are kv3<sup>5</sup></sup></sub> \
<sub><sup>Some early versions of v125 is kv3, however newer versions are kv4<sup>5</sup></sup></sub> \
<sub><sup>v124 or lower recommended<sup>5</sup></sup></sub>

# How do I unenroll?
## ChromeOS v101 and below - SHroot
Pretty cool universal USB-less unenrollment exploit that requires you to have a Chromebook that hasn't been touched in years. It exploits a bash shell and root escalation method in Crosh that has been long patched, but still very neat and very easy.

1. Login to your Chromebook.
2. Open Crosh with `ctrl+alt+t`. If it says `crosh is blocked`, use SH1mmer.
3. Paste the following in:

`set_cellular_ppp \';dbus-send${IFS}--system${IFS}--print-reply${IFS}--dest=org.chromium.SessionManager${IFS}/org/chromium/SessionManager${IFS}org.chromium.SessionManagerInterface.ClearForcedReEnrollmentVpd;exit;\'`

Just like this \
<img src="https://github.com/kkilobyte/ditch-cros/blob/main/img/tutorial/crosh-rootesc.png?raw=true" alt="crosh-rootesc.png"/><

5. Press `enter`.
6. Back up all data to an external media (like a USB flash drive) or a cloud service (like Google Drive).
7. Press `esc+⟳+⏻ ` (`esc+refresh+power`).
8. Press `ctrl+d` and then `enter`.
9. On the scary screen with black text at the top left, press `enter` again.
10. Wait for ChromeOS to boot, and then go through the setup.
11. Now you can set up ChromeOS with a personal Google account.

## ChromeOS v110 and below - SH1mmer
The preferred unenrollment method for ChromeOS v110 and below is using SH1mmer's very cool "deprovision" option. This takes ownership of the TPM and erases the FWMP, along with making ChromeOS not check for enrollment by putting a parameter in the RW portion of the VPD.

1. Back up all data to an external storage device or cloud service. You must not use the same storage device as the device you will be using SH1mmer with.
2. If you are using ChromeOS, MacOS, or Windows, download [this extension](https://chromewebstore.google.com/detail/chromebook-recovery-utili/pocpnlppkickgojjlmhdmidojbmbodfm). (Linux users skip this step.) If you only have the Chromebook and this is blocked, try using [Skiovox](https://skiovox.com/skiovox.pdf). If you are using a version where Skiovox is patched, you can't use SH1mmer anyways.
3. [Identify](/device-identify.md) what Chromebook *board* you have.
4. Find and download your *board*'s SH1mmer [here](https://dl.darkn.bio/SH1mmer/Prebuilt/Legacy), if it's missing, good luck, use BadRecovery or CRSH2TTY.
5. Open your "Downloads" folder in the "Files" app, double click on the SH1mmer zip file, and drag the SH1mmer bin file to your Downloads folder.
6. If you are using Linux, skip to step 8, otherwise open a Chrome tab, click on the puzzle icon in the top right, and click on "Chromebook Recovery Utility".

<img src="/img/tutorial/chrome-recovery-extension.png">
7. Click on the ⚙ (settings) icon in the corner and click "Use local image" and the select your SH1mmer bin file.
<img src="/img/tutorial/cru-local-image.png">

8. If you don't use Linux, skip to step 9, otherwise, open a terminal and run `lsblk` or `fdisk -l` and verify what your USB drive is, once you have verified, run `cd ~/Downloads; sudo dd if=<sh1mmer file> of=/dev/sd<usb letter> oflag=direct status=progress bs=16M` and wait. Skip to step 10.
9. Plug in the USB drive that you want to use for SH1mmer, do ***NOT*** use the USB drive with your data if you backed up data to a USB drive.
10. Verify this USB drive doesn't have important data and then wait for it to flash.
11. Once finished, press `esc+⟳+⏻ ` (`esc+refresh+power`) and then `ctrl+d`. Then press `esc+⟳+⏻ ` (`esc+refresh+power`) again and insert the USB.
12. Wait for SH1mmer to load, once you are greeted with a scary menu with lots of options.
<img src="/img/tutorial/sh1mmer.jpg" width="400">

13. Press `d` and then `enter`, this should come with two messages both having "SUCCESS!". Once it says "FINISHED", preform an EC reset by pressing `⟳+⏻ ` (`refresh+power`).
14. You should be greeted at a "OS Verification is off" screen but with no black text in the corner. Press `ctrl+d` whenever you Chromebook turns on and you see this screen. 
15. Setup ChromeOS as normal
16. Now you can set up ChromeOS with a personal Google account.
