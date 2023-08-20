

#### Step 1: Open a terminal 

#### Step 2: Update and upgrade system packages

Note: If your Linux distro does not fall into one of options listed
below, you will need to research how to `update` and `upgrade` your system
packages.

- Option for Debian based distributions such as Ubuntu, Kali, Armbian and Raspberry Pi OS

```
sudo apt update && sudo apt upgrade
```

- Option for Arch based distributions such as Manjaro

```
sudo pacman -Syu
```

- Option for Fedora based distributions

```
sudo dnf upgrade
```

- Option for openSUSE based distributions

```
sudo zypper update
```

- Option for Void Linux

```
sudo xbps-install -Syu
```

Note: It is recommended that you reboot your system at this point. The
rest of the installation will appreciate having a fully up-to-date
system to work with. The installation can then be continued with Step 3.

```
sudo reboot
```

#### Step 3: Install the required packages (select the option for the distro you are using)

Note: If your Linux distro does not fall into one of options listed
below, you will need to research how to properly setup up the development
environment for your system. General guidance follows.

Development Environment Requirements: (package names may vary by distro)

- Mandatory packages: `gcc` `make` `bc` `kernel-headers` `build-essential` `git`
- Highly recommended packages: `dkms` `rfkill` `iw` `ip`
- Mandatory packages if Secure Boot is active: `openssl` `sign-file` `mokutil`

Note: The below options should take care of the mandatory and highly recommended
requirements. If Secure Boot is active on your system, please also install the
mandatory packages for Secure Boot as shown above.

- Option for Armbian (arm64)

```
sudo apt install -y build-essential
```

- Option for Raspberry Pi OS (arm/arm64)

```
sudo apt install -y raspberrypi-kernel-headers build-essential bc dkms git
```

- Option for Debian, Kali, and Raspberry Pi Desktop (x86)

```
sudo apt install -y linux-headers-$(uname -r) build-essential bc dkms git libelf-dev rfkill iw
```

- Option for Ubuntu (all official flavors) and the numerous Ubuntu based distros

```
sudo apt install -y build-essential dkms git iw
```

- Option for Fedora

```
sudo dnf -y install git dkms kernel-devel
```

- Option for openSUSE

```
sudo zypper install -t pattern devel_kernel dkms
```

- Option for Alpine

```
sudo apk add linux-lts-dev make gcc
```

- Option for Void Linux

```
sudo xbps-install linux-headers dkms git make
```

- Options for Arch and Manjaro (if using Manjaro for RasPi4B, see note)

If using pacman

```
sudo pacman -S --noconfirm linux-headers dkms git bc iw
```

Note: The following is needed if using Manjaro for RasPi4B.

```
sudo pacman -S --noconfirm linux-rpi4-headers dkms git bc
```

Note: If you are asked to choose a provider, make sure to choose the one
that corresponds to your version of the linux kernel (for example,
"linux510-headers" for Linux kernel version 5.10). If you install the
incorrect version, you'll have to uninstall it and install the correct
version.

If using other methods, please follow the instructions provided by those
methods.

#### Step 4: Create a directory to hold the downloaded driver

```
mkdir -p ~/src
```

#### Step 5: Move to the newly created directory

```
cd ~/src
```

#### Step 6: Download the driver

```
git clone https://github.com/morrownr/8814au.git
```

#### Step 7: Move to the newly created driver directory

```
cd ~/src/8814au
```

#### Step 8: Run the installation script (`install-driver.sh`)

Note: It is recommended that you terminate running apps so as to provide the
maximum amount of RAM to the compilation process.

Note: For automated builds (non-interactive), use `NoPrompt` as an option.

```
sudo ./install-driver.sh
```

or

```
sudo sh install-driver.sh
```

Note: If you elect to skip the reboot at the end of the installation
script, the driver may not load immediately and the driver options will
not be applied. Rebooting is strongly recommended.

Note: Fedora users that have secure boot turned on may need to run the
following to enroll the key:

```
sudo mokutil --import /var/lib/dkms/mok.pub
```

### Manual Installation Instructions

Note: The above installation steps automate the installation process,
however, if you want to or need to do a basic command line installation,
use the following:

```
make clean
```

```
make
```

If secure boot is off:

```
sudo make install
```

```
sudo reboot
```

If secure boot is on:

Note: Please read to the end of this section before coming back here to
enter commands.

```
sudo make sign-install
```

You will be promted for a password, please remember the password as it
will be used in some of the following steps.

```
sudo reboot
```

The MOK managerment screen will appear during boot:

`Shim UEFI Key Management"

`Press any key...`

Select "Enroll key"

Select "Continue"

Select "Yes"

When promted, enter the password you entered earlier.

If you enter the wrong password, your computer will not be bootable. In
this case, use the BOOT menu from your BIOS to boot then as follows:

```
sudo mokutil --reset
```

Restart your computer and use the BOOT menu from BIOS to boot. In the MOK
managerment screen, select `reset MOK list`. Then Reboot and retry from
the step `sudo make sign-install`.

To remove the driver if installed by the manual installation instructions:

```
sudo make uninstall
```

```
sudo reboot
```

Note: If you use the manual installation instructions, you will need to
repeat the process each time a new kernel is installed in your distro.

-----

### Driver Options (`edit-options.sh`)

A file called `8814au.conf` will be installed in `/etc/modprobe.d` by
default if you use the `install-driver.sh` script.

Note: The installation script will prompt you to edit the options.

Location: `/etc/modprobe.d/8814au.conf`

This file will be read and applied to the driver on each system boot.

To edit the driver options file, run the `edit-options.sh` script

```
sudo ./edit-options.sh
```

Note: Documentation for Driver Options is included in the file `8814au.conf`.

-----

### Upgrading the Driver

Note: Linux development is continuous therefore work on this driver is continuous.

Note: Upgrading the driver is advised in the following situations:

- if a new or updated version of the driver needs to be installed
- if a distro version upgrade is going to be installed (i.e. going from kernel 5.10 to kernel 5.15)

#### Step 1: Move to the driver directory

```
cd ~/src/8814au
```

#### Step 2: Remove the currently installed driver

```
sudo ./remove-driver.sh
```

#### Step 3: Pull updated code from this repo

```
git pull
```

#### Step 4: Install the driver

```
sudo ./install-driver.sh
```

-----
### Removal of the Driver (`remove-driver.sh`)

Note: Removing the driver is advised in the following situations:

- if driver installation fails
- if the driver is no longer needed

Note: The following removes everything that has been installed, with the
exception of the packages installed in Step 3 and the driver directory.
The driver directory can be deleted after running this script.

#### Step 1: Open a terminal (e.g. Ctrl+Alt+T)

#### Step 2: Move to the driver directory

```
cd ~/src/8814au
```

#### Step 3: Run the removal script

Note: For automated builds (non-interactive), use `NoPrompt` as an option.

```
sudo ./remove-driver.sh
```

-----

