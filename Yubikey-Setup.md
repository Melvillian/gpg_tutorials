# Setup

## Install required software

### OSX

GUI Only: https://developers.yubico.com/yubioath-desktop/Releases/

GUI + CLI Tools: 
```
brew tap homebrew/versions
brew install gpg ykpers pinentry pinentry-mac swig python cmake pyside python3 libu2f-host libusb
pip3 install yubikey-manager
echo "pinentry-program $(which pinentry-mac)" >> $HOME/.gnupg/gpg-agent.conf
echo "PATH=$HOME/Library/Python/2.7/bin:$PATH" >> $HOME/.bash_profile
source .bash_profile
ln -s /usr/local/bin/gpg2 /usr/local/bin/gpg
```

### Windows

GUI Only: https://developers.yubico.com/yubioath-desktop/Releases/

GUI + CLI Tools: 
```
choco install pip yubikey-personalization-tool gpg4win openssh
pip install --user yubioath-desktop
```

### Linux

#### Arch

```
pacman -S gpg yubikey-personalization pcsc-tools pcsclite libusb-compat libu2f-host swig gcc python2-pyside python2-click
sudo systemctl enable --now pcscd.service
pip install --user yubioath-desktop
```

#### Debian

```
apt-get install yubikey-personalization yubikey-personalization-gui gpgv2 pinentry-gtk2 swig pyside python-pip
pip install --user yubioath-desktop 
```

#### Ubuntu

```
sudo add-apt-repository ppa:yubico/stable
sudo apt-get update
apt-get install yubikey-personalization yubikey-personalization-gui gpgv2 pinentry-gtk2 swig pyside python-pip
pip install --user yubioath-desktop
```

## Factory Reset Yubikey (Optional)

If you have an existing Yubikey that you wish to wipe and re-use, enter the following commands.

```
gpg --card-edit
gpg/card> admin
gpg/card> factory-reset
Continue? (y/N) y
Really do a factory reset? (enter "yes") yes
```

## Set PIN

To proceed with GPG operations you must set user/admin pin codes for your key. It is recommended these be different. 
You will use the User pin day to day for things like SSH or GPG but you will only use the Admin pin in the event you lock yourself out or to change certain protected settings.

```
gpg2 --change-pin
> 3 # change Admin PIN
> 1 # change User PIN 
```

## Set flags (Yubikey Only)

The following will enable security features on the Yubikey that are only relevant to engineers/developers and are not needed for browser-only workflows.

### Yubikey Neo / Yubikey Neo-n

Enable GPG mode:
```
ykpersonalize -m86
Firmware version 3.4.2 Touch level 1541 Program sequence 1

The USB mode will be set to: 0x86

Commit? (y/n) [n]: y
```

### Yubikey 4 / Yubikey 4 Nano

Require physical touch for all key operations. After each command, try running it again in order to verify the operation
(sig, aut, enc) were correctly set to ON_FIXED:

```
ykman openpgp touch sig fixed
ykman openpgp touch aut fixed
ykman openpgp touch enc fixed
```
