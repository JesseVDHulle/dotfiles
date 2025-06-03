# OS Installation
Here you find my process of installing my OS to get started.

## Arch Linux
My main OS to go is *Arch Linux*. 

1. Install the latest [arch linux ISO](https://archlinux.org/download/) and flash this onto a USB drive (+2GB), e.g. using rufus or balena-etcher.

2. Boot into the USB stick. You might need to check boot order and USB boot in BIOS.

3. Make sure you are connected to the internet and make sure you got the latest packages installed.
    ```sh
    pacman -Sy
    pacman -S archlinux-keyring
    pacman -S archinstall
    ```


4. **Partioning:** Partition the disk in 4 parts
    - UEFI System: 1GB - fat -F32
    - Linux Swap: 16GB **In this case, double size of physical RAM*
    - Linux Filesystem (root): 100-400 GB - btrfs
    - Linux Filesystem (home): Rest - ext4
    ```sh
    cfdisk /dev/DEVICE_NAME
    ```

    Assign filesystems:
    ```sh
    mkfs.fat -F32 /dev/PARTITION_NAME
    mkfs.btrfs /dev/PARTITION_NAME
    mkfs.ext4 /dev/PARTITION_NAME
    mkswap /dev/PARTITION_NAME
    ```

    Mount Partitions
    ```sh
    mount /dev/ROOT_PARTITION /mnt
    mkdir /mnt/boot /mnt/home
    mount /dev/BOOT_PARTITION /mnt/boot
    mount /dev/HOME_PARTITION /mnt/home
    swapon /dev/SWAP_PARITION
    ```


5. **Archinstall:** Run the archinstall command and go through every option
    ```sh
    archinstall
    ```

    > **NOTE:** The current using archinstall configuration can be found in the folder OS under [user_configuration.json](archinstall/user_configuration.json)

    Special mentions:
    * Optional repositories: Enable *multilib* (for gaming purposes)
    * Disk configuration: Select *pre-mounted* on mountpoint */mnt*
    * Swap: *Disabled* - already made this a partition
    * Bootloader: *Grub* - Used with custom theme and for dual booting
    * Profile: *minimal* - we install the desktop later
    * Audio: *pipewire* - prefered
    * Network configuration: *NetworkManager* - prefered<br>
    * Additional packages: linux-headers nano vim
    Finally hit *Install* and boot into your system!
    <br>
6. *(optional)* **Dual booting:** Assuming you got a different os on a different device
    In this step we initlialize grub to also boot into windows.

    1. Change default Grub file
    ```sh
    sudo nano /etc/default/grub
    ```
    2. Uncomment `#GRUB_DISABLE_OS_PROBER`

    3. Install os-prober
    ```sh
    sudo pacman -S os-prober
    ```

    4. Mount Windows boot manager
    ```sh
    sudo mount /dev/BOOT_PARTITION /mnt
    ```
    > NOTE: You can check the windows boot manager with fdisk -l

    5. Find windows Boot Manager by os-prober.
    Go to the boot directory of windows and run os-prober
    ```sh
    cd /mnt/EFI/Microsoft
    sudo os-prober
    ```

    6. Update grub file
    ```sh
    sudo grub-mkconfig -o /boot/grub/grub.cfg
    ```

    When you reboot now, windows should appear in the Grub menu

Refer to the [Arch Wiki](https://wiki.archlinux.org/title/Archinstall) for more details.

---
## Post Installation

First make sure to update the system
```sh
sudo pacman -Syu
```

Standard packages to install using Pacman
```sh
sudo pacman -S base-devel wget curl man-db man-pages
 pkgfile git fastfetch htop amd-ucode zsh
```

### Package Manager
Alter pacman configuration
```sh
sudo nano /etc/pacman.conf
```
Uncomment *Color*, *ParallelDownloads* and add *ILoveCandy* to the Misc options

Backup current mirrorlist and update mirrorlist using reflector for using the fastest mirror servers.
```sh
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
sudo pacman -S reflector
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
sudo pacman -Sy
```

AUR Support using yay
```sh
git clone https://aur.archlinux.org/yay.git
cd yay && makepkg -si
```

### Graphics Driver  
*Nvidia GPU*
Install the correct drivers.
I recommend the [guide](https://github.com/korvahannu/arch-nvidia-drivers-installation-guide) by korvahannu
In this guide it shows you which drivers to install and how to update the bootloaders acoording to the nvidia drivers.

You'll end up installing something like this:
```sh
sudo pacman -S nvidia-open nvidia-utils lib32-nvidia-utils
```

*Hyprland (optional)*
Since hyprland works best with AMD GPUs there need to be some extra configuration for Nvidia. Please refer to (https://wiki.hyprland.org/Nvidia/)
After following the guide, these environment variables need to be kept track of to add later to the Hyprland config:
* env = LIBVA_DRIVER_NAME,nvidia
* env = __GLX_VENDOR_LIBRARY_NAME,nvidia
* env = ELECTRON_OZONE_PLATFORM_HINT,auto
* env = NVD_BACKEND,direct

### Backup options  
Install second LTS kernel
```sh
sudo pacman -S linux-lts linux-lts-headers
```
Make sure to update Grub menu to always default to normal linux.
```sh
sudo nano /etc/default/grub
```
and change the following`GRUB_DEFAULT=Arch Linux, with Linux linux` and `GRUB_DISABLE_SUBMENU=y`

System backuping using timeshift
```sh
sudo yay -Sy timeshift
```
TODO: And configure the timeshift in the UI

---
## GUI
### Hyperland
Too install hyprland consult the offical [hyprland docs](https://wiki.hyprland.org/Getting-Started/)

Other packages to install for compatibility
```sh
sudo pacman -S xdg-desktop-portal-hyprland polkit-kde-agent qt5-wayland qt6-wayland dunst jq gnome-keyring neovim
```

Also make sure to install the following apps:
```sh
sudo pacman -S kitty thunar firefox
```

To make sure you installed fonts used by application, install the following:
```sh
sudo pacman -S ttf-dejavu ttf-liberation noto-fonts noto-fonts-emoji
fc-cache -fv
```

### KDE Plasma
> The second choice is KDE Plasma [latest](https://kde.org/download/). However I don't have a pre-defined config for this yet.