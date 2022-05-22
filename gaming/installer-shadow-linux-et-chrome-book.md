# Installer Shadow Linux et Chrome book

Pour installer l'application Linux, téléchargez l'AppImage du site, mettez les droits d'exécution, mettez la dans un endroit confortable et lancez là. Nous vous conseillons l'AppImage plutôt qu'un autre packaging. Les AppImage sont accessibles depuis votre espace membre. Pour installer votre GPU correctement, suivez les informations.

## Correctif 1

\
Pour ceux présentant un stream coloré du plus bel effet (ex: que des variantes de rouge), ceci est dû à une mauvaise utilisation des drivers par votre OS. Pour le régler, exécutez la commande suivante, et relancez l'application.

```
curl https://gitlab.com/NicolasGuilloux/shadow-live-os/raw/arch-master/airootfs/etc/drirc -o ~/.drirc
```

&#x20;Plus d'info ici : [https://nicolasguilloux.github.io/blade-shadow-beta/issues#the-drirc-fix](https://nicolasguilloux.github.io/blade-shadow-beta/issues#the-drirc-fix)\
Source  : Discord Shadow

## Correctif 2

Une nouvelle mise à jour a eu lieu hier, le 26 août, et apporte le Quick Menu. Si vous rencontrez des problèmes, merci bien vouloir : 1. Vérifier que `libva-glx2` est installé sur votre machine 2. Si vous rencontrez toujours des problèmes, veuillez activer l'option "Faible Configuration" (et non faible connexion !). Celle-ci va désactiver le Quick Menu, il est possible que votre GPU ne le supporte pas. 3. Partagez nous le lien retourné par cette commande : `curl https://raw.githubusercontent.com/NicolasGuilloux/blade-shadow-beta/master/scripts/report.pl | perl` 4. Pinguez moi ou un dev qu'on récolte vos rapports\
\
Source  : Discord Shadow

## Installation

Shadow is compatible with the following OS and their more recent versions:

[Ubuntu 18.04](https://ubuntu.com/), [Linux Mint 19](https://www.linuxmint.com/), [Debian 10](https://www.debian.org/), [Arch Linux](https://www.archlinux.org/), [Solus](https://getsol.us/), [Fedora 28](https://getfedora.org/), [GalliumOS 3.0](https://galliumos.org/).

If your OS meets the minimum version, lets get started! Please [download the official AppImage](https://nicolasguilloux.github.io/blade-shadow-beta/index#appimage).

You may pick the one you prefer based on your liking but keep in mind that the Linux app is not under very active development, which means sometimes it may break even in Stable and a fix may be deployed only on Alpha or Beta. We recommand your to download them all, and change if you meet a blocking bug.

You now have the AppImage. Make it executable and start it to check that the launcher is here. Sadly, it is rarely click'n'play.

**Shadow is not compatible with Wayland!** Use Xorg instead.

**For every user who wants the remember me feature**, you need to install [`gnome-keyring`](https://wiki.archlinux.org/index.php/GNOME/Keyring).

**On Arch Linux**, Shadow requires [`libsndio-61-compat`](https://aur.archlinux.org/packages/libsndio-61-compat/).

**For Fedora users**, you may need to install the library [`librtmp`](https://pkgs.org/search/?q=librtmp).

**For Ubuntu 20.04+ users**, you may need to install the following driver for Intel GPUs [`intel-media-va-driver-non-free`](https://packages.ubuntu.com/search?keywords=intel-media-va-driver-non-free).

The community built a wonderful report script that gather all information to debug quickly your setup. Everytime you ask for help, please join the link given back the following command:

`curl https://raw.githubusercontent.com/NicolasGuilloux/blade-shadow-beta/master/scripts/report.pl | perl`

## Setup you GPU for the VA-API <a href="#setup-you-gpu-for-the-va-api" id="setup-you-gpu-for-the-va-api"></a>

As Shadow for Linux uses the VA-API, it is important to well setup your GPU for it. So it is important to understand how works`vainfo` and how to debug it.

First things first, vainfo needs to be installed on your OS. It is a program that describes how your GPU is detected by the VA-API.

* Debian: `sudo apt install libva-glx2 vainfo`
* Arch Linux: `sudo pacman -S libva-utils`
* Solus: `sudo eopkg it libva-utils`

Vainfo scans your GPU with the given driver to check his abilities. There are 3 things to check from the result of the command:

* If it returns nothing or returns -1 somewhere, it means that the GPU is not well detected. Your VA driver needs to be updated.
* In the list of profiles, it mentions "H264" at least once. If supported, you can use the Shadow on Linux.
* In the list of profiles, it mentions "H265" or "HEVC" at least once. If not, you will not be able to use H265 but as long as you have H264, you still can use Shadow on Linux.

The following result show that the GPU is well recognized and have both profiles wanted. The relevant information are highlighted.

`libva info: VA-API version 1.1.0`\
`libva info: va_getDriverName() returns 0`\
`libva info: Trying to open /usr/lib/x86_64-linux-gnu/dri/i965_drv_video.so`\
`libva info: Found init function __vaDriverInit_1_1`\
`libva info: va_openDriver() returns 0`\
`vainfo: VA-API version: 1.1 (libva 2.1.0)`\
`vainfo: Driver version: Intel i965 driver for Intel(R) Skylake - 2.1.0`\
`vainfo: Supported profile and entrypoints`\
&#x20;`VAProfileMPEG2Simple : VAEntrypointVLD`\
&#x20;`VAProfileMPEG2Simple : VAEntrypointEncSlice`\
&#x20;`VAProfileMPEG2Main : VAEntrypointVLD`\
&#x20;`VAProfileMPEG2Main : VAEntrypointEncSlice`\
&#x20;`VAProfileH264ConstrainedBaseline: VAEntrypointVLD`\
&#x20;`VAProfileH264ConstrainedBaseline: VAEntrypointEncSlice`\
&#x20;`VAProfileH264ConstrainedBaseline: VAEntrypointEncSliceLP`\
&#x20;`VAProfileH264ConstrainedBaseline: VAEntrypointFEI`\
&#x20;`VAProfileH264ConstrainedBaseline: VAEntrypointStats`\
&#x20;`VAProfileH264Main : VAEntrypointVLD`\
&#x20;`VAProfileH264Main : VAEntrypointEncSlice`\
&#x20;`VAProfileH264Main : VAEntrypointEncSliceLP`\
&#x20;`VAProfileH264Main : VAEntrypointFEI`\
&#x20;`VAProfileH264Main : VAEntrypointStats`\
&#x20;`VAProfileH264High : VAEntrypointVLD`\
&#x20;`VAProfileH264High : VAEntrypointEncSlice`\
&#x20;`VAProfileH264High : VAEntrypointEncSliceLP`\
&#x20;`VAProfileH264High : VAEntrypointFEI`\
&#x20;`VAProfileH264High : VAEntrypointStats`\
&#x20;`VAProfileH264MultiviewHigh : VAEntrypointVLD`\
&#x20;`VAProfileH264MultiviewHigh : VAEntrypointEncSlice`\
&#x20;`VAProfileH264StereoHigh : VAEntrypointVLD`\
&#x20;`VAProfileH264StereoHigh : VAEntrypointEncSlice`\
&#x20;`VAProfileVC1Simple : VAEntrypointVLD`\
&#x20;`VAProfileVC1Main : VAEntrypointVLD`\
&#x20;`VAProfileVC1Advanced : VAEntrypointVLD`\
&#x20;`VAProfileNone : VAEntrypointVideoProc`\
&#x20;`VAProfileJPEGBaseline : VAEntrypointVLD`\
&#x20;`VAProfileJPEGBaseline : VAEntrypointEncPicture`\
&#x20;`VAProfileVP8Version0_3 : VAEntrypointVLD`\
&#x20;`VAProfileVP8Version0_3 : VAEntrypointEncSlice`\
&#x20;`VAProfileHEVCMain : VAEntrypointVLD`\
&#x20;`VAProfileHEVCMain : VAEntrypointEncSlice`

## Install your drivers <a href="#install-your-drivers" id="install-your-drivers"></a>

Now that you understand how vainfo works, it is time to install the appropriate driver for your GPU. Sadly, there is not one driver and it depends on your GPU brand and model.

As it may change during time, we recommand you to read the documentation of the 2 main Linux distributions, especially the Arch Linux wiki because it is very complete: [Arch Linux](https://wiki.archlinux.org/index.php/Hardware\_video\_acceleration#Installation), [Ubuntu](https://doc.ubuntu-fr.org/vaapi).

But as we are so nice, the following table is a summary of all the main cases we encountered. If you meet some difficulties to install the drivers, please come in the official Discord so we can speak and let us help you.

#### Intel GPU

| OS         | < 8th gen          | > 8th gen                      |
| ---------- | ------------------ | ------------------------------ |
| Ubuntu     | i965-va-driver     | intel-media-va-driver-non-free |
| Arch Linux | intel-media-driver | libva-intel-driver             |
| Solus      | libva-intel-driver | x                              |
| NixOS      | vaapiIntel         | intel-media-driver             |

#### AMD GPU

| OS         | Radeon HD xxxx    | Radeon Rxxx     |
| ---------- | ----------------- | --------------- |
| Ubuntu     | mesa-va-drivers   | vdpau-va-driver |
| Arch Linux | libva-mesa-driver | mesa-vdpau      |
| Solus      | x                 | libvdpau-va-dl  |
| NixOS      | vaapiVdpau        | libvdpau-va-dl  |

#### NVIDIA GPU

| OS         | NVIDIA < 800 series | NVIDIA > 800 series                                                     |
| ---------- | ------------------- | ----------------------------------------------------------------------- |
| Ubuntu     | x                   | Check [Arekinath's patch](https://gitlab.com/aar642/libva-vdpau-driver) |
| Arch Linux | nouveau-fw          |                                                                         |
| Solus      | x                   | x                                                                       |
| NixOS      | x                   | x                                                                       |



Source : [https://nicolasguilloux.github.io/blade-shadow-beta/setup](https://nicolasguilloux.github.io/blade-shadow-beta/setup)



## Installation chrome book

Tutoriel pour installer l'application Linux sur un Chromebook par @Mat€o Ce tutoriel est avancé mais bien expliqué. Pour toute question, hésitez pas à vous adresser à lui ![:wink:](https://discord.com/assets/2e41bfdeba797283ee9da9bb439c3ece.svg) Pour sélectionner Linux au boot, faites `CTRL + ALT + MAJ + Retour arrière` Préférez le Log out lorsque vous quittez Linux

Source : Discord Shadow

{% file src="../.gitbook/assets/2020shadow_sur_chromebook.pdf" %}
