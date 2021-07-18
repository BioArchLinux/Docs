
## Can't Sign the GPG Key

BioArchLinux uses aur-build to build my local repository via GitHub Action.

To easy understand the process of using it, I write an [English Guide](https://github.com/BioArchLinux/aur-bui … r-build.md) for it.

But things always occur on makepkg  by GitHub Action. They always show `makepkg --noprogressbar --force --nocolor fail.`

```
==> Leaving fakeroot environment.
==> Signing package(s)...
==> WARNING: Failed to sign package file beast2-2.6.4-1-x86_64.pkg.tar.zst.

Command 'makepkg --noprogressbar --force --nocolor' failed to execute.
:: Removing already installed dependencies for beast2:
=> sudo /tmp/pacman --color=never --remove giflib libpng harfbuzz nspr lcms2 jre-openjdk libnet libtiff java-environment-common openjdk-src freetype2 libjpeg-turbo openjdk-doc nss hicolor-icon-theme graphite jdk-openjdk jre-openjdk-headless java-runtime-common
25lchecking dependencies...
warning: dependency cycle detected:
warning: freetype2 will be removed after its harfbuzz dependency

Packages (19) freetype2-2.10.4-1  giflib-5.2.1-2  graphite-1:1.3.14-1
              harfbuzz-2.8.2-1  hicolor-icon-theme-0.17-2
              java-environment-common-3-3  java-runtime-common-3-3
              jdk-openjdk-16.0.1.u9-1  jre-openjdk-16.0.1.u9-1
              jre-openjdk-headless-16.0.1.u9-1  lcms2-2.12-1
              libjpeg-turbo-2.1.0-1  libnet-1:1.1.6-1  libpng-1.6.37-3
              libtiff-4.3.0-1  nspr-4.32-1  nss-3.68-1  openjdk-doc-16.0.1.u9-1
              openjdk-src-16.0.1.u9-1

Total Removed Size:  611.70 MiB

:: Do you want to remove these packages? [Y/n] 
Y
:: Processing package changes...
removing openjdk-doc...
removing openjdk-src...
removing jdk-openjdk...
removing hicolor-icon-theme...
removing java-environment-common...
removing jre-openjdk...
removing jre-openjdk-headless...
removing java-runtime-common...
removing nss...
removing libnet...
removing lcms2...
removing libtiff...
removing libjpeg-turbo...
removing nspr...
removing harfbuzz...
removing graphite...
removing freetype2...
removing libpng...
removing giflib...
:: Running post-transaction hooks...
(1/1) Arming ConditionNeedsUpdate...

Can't build 'beast2'.

Failed to build following packages:
beast2
```

So, I am wondering if `makepkg --noprogressbar --force --nocolor` need some special condition or running environment, or any other reasons?
I think the GPG key is the main problem.
In GitHub Action, and it shows

```
gpg: next trustdb check due at 2021-08-02
gpg: key EC035DFB7F9A2A8A: public key "BioArchLinux <BioArchLinux@malacology.net>" imported
gpg: Total number processed: 1
 gpg:               imported: 1
 -> Locally signed 1 keys.
==> Updating trust database...
[LOG] Importing GPG
gpg: directory '/home/aur-build/.gnupg' created
gpg: keybox '/home/aur-build/.gnupg/pubring.kbx' created
gpg: /home/aur-build/.gnupg/trustdb.gpg: trustdb created
gpg: key EC035DFB7F9A2A8A: public key "BioArchLinux <BioArchLinux@malacology.net>" imported
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key EC035DFB7F9A2A8A: secret key imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1
OK
```

So it just shows that this GPG key is imported. but things are strange when run `makepkg --noprogressbar --force --nocolor`, input the right password and then it shows Failed to sign package.

I use this script to run it
```
  LOG "Initing GPG"
  rm -fr /etc/pacman.d/gnupg
  pacman-key --init
  pacman-key --populate archlinux
  pacman-key --recv-keys $GPGKEY --keyserver hkp://keyserver.ubuntu.com
  pacman-key --lsign-key $GPGKEY
```

I can use `pacman-key --recv-keys $GPGKEY --keyserver hkp://keyserver.ubuntu.com` and `pacman-key --lsign-key $GPGKEY` on my PC (ArchLinux) well and no error is shown.

All the log is shown here.

https://github.com/BioArchLinux/testing … 40004/logs
