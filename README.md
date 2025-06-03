# Dotfiles

A collection of my personal configuration files and scripts for my Linux environment.
Follow this guide to install my personal config files.
See the guide for [OS installation](OS_INSTALLATION) for how to set up my linux environment.

## Installation
TODO:
1. **Clone the repository:**
    ```sh
    git clone https://github.com/yourusername/dotfiles.git
    ```

2. **Run the install script:**
    ```sh
    cd ./dotfiles
    ./install.sh
    ```

3. **Follow any prompts to complete setup.**

---

## Configuration

Theme
TODO: Not working with nautilus
To make sure applications will use the desired dark theme, we install a gtk themer.
```sh
pacman -S nwg-look
pacman -S qt5ct qt6ct kvantum
yay -S kvantum-theme-catppuccin-git
nwg-look
```

Hyperland Config

Hyprlock Config

Waybar Config

ZSH Config
```sh
sudo pacman -S zsh-autosuggestions zsh-completions zsh-syntax-highlighting
```

```sh
yay -S oh-my-posh
``` 

Kitty Config

Greetd + Tuigreet Config

Firefox Config

Nautilus Config

Dunst Config

Rofi Config

ClipBoard Config

LogOut/ShutdownManager

ScreenshotManager

Show/Hide desktop shortcut

Firewall Config
```sh
sudo pacman -Syu ufw
sudo ufw enable
sudo systemctl enable ufw 
```

Gemini Config

OpenRGB Config

BossKatana Config

Rofi Scripts

TODO:
- All configuration files are stored in the `configs/` directory.
- Symlinks are created to your home directory (e.g., `~/.bashrc`, `~/.vimrc`).
- Edit the configs as needed before or after installation.

---

## Usage
TODO:
- Scripts are located in the `scripts/` directory.
- Add scripts to your `PATH` or run them directly:
  ```sh
  ./scripts/example.sh
  ```

---

## Customization
TODO:
- Modify any config file to suit your preferences.
- Add new scripts or configs as needed.
- Pull requests are welcome!

---

## License

This project is licensed under the [MIT License](LICENSE).
