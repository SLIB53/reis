# Reis

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
