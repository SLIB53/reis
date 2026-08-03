# Reis

Reis is a homelab setup for running CUDA workloads on a Razer Blade Laptop. The bare-metal installation maintains a dual-boot (Windows 11 and Ubuntu 26.04 LTS), and preinstalls all of my wanted configurations and tools.

## Storage

The Ubuntu installation is on a BTRFS partition encrypted with LUKS, and a ZFS pool was also created on the side for storage. 


## Customization

### Set Screen Brightness

This requires NVIDIA Dynamic Display Switch to be on in the BIOS.

```sh
echo 1 | sudo tee /sys/class/backlight/*/brightness
```


### Quiet SSH

#### Disable MOTD

Edit the PAM configuration for OpenSSH.

```sh
sudo vi /etc/pam.d/sshd  # INTERVENE: Look for optional session configurations you'd like to disable.
```


#### Hush Login

```sh
touch ~/.hushlogin
```


### macOS Parity

#### SSH Configuration

Add reis as a host to `~/.ssh/config`, and remember to set forward COLORTERM. By default, Ubuntu accepts this in its `/etc/ssh/sshd_config`.

```ssh
Host reis
	HostName # CHANGEME
	User # CHANGEME
	ServerAliveInterval 60
	ServerAliveCountMax 3
	SendEnv COLORTERM
```


#### Configure Fish Theme & Keybindings

Fish themes vary between macOS and Ubuntu, mainly in their case. For example, `mono-smoke` is `Mono Smoke` on Ubuntu. Further, the opt key behaves slightly differently from the macOS default behavior.

```fish
fish_config theme choose "Mono Smoke"
```

```fish
bind alt-left backward-word
bind alt-right forward-word
bind alt-backspace backward-kill-word
```


#### Share Helix Configurations

```sh
scp ~/.config/helix/config.toml reis:/home/akil/.config/helix/
```

```sh
scp -r ~/.config/helix/themes/ reis:/home/akil/.config/helix/
```

**Note:** Before sharing helix configurations, a quick reminder that terminal apps may need a configuration change to send the appropriate character for opt/cmd. For example, the following configures Ghostty:

```ini
# Opt+arrows: send real Alt+Arrow instead of ESC b/f
keybind = alt+left=csi:1;3D
keybind = alt+right=csi:1;3C
keybind = shift+alt+left=csi:1;4D
keybind = shift+alt+right=csi:1;4C

# Cmd+arrows: send Home/End
keybind = super+left=csi:H
keybind = super+right=csi:F
keybind = shift+super+left=csi:1;2H
keybind = shift+super+right=csi:1;2F
```
