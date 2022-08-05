# Raspberry Pi 3 Config
This is just my configuration for my Raspberry Pi 3 for:-
- Running Emby/Jellyfin as media server
- Mounting an external HDD

## OS
Raspberry Pi OS with desktop.

Download link:- https://www.raspberrypi.com/software/operating-systems/

## Update Linux packages and kernel
```
sudo apt-get update
sudo apt-get upgrade
sudo apt full-upgrade
sudo rpi-update
sudo reboot
```


## Install Emby
```
cd Desktop

wget https://github.com/MediaBrowser/Emby.Releases/releases/download/4.7.5.0/emby-server-deb_4.7.5.0_armhf.deb

dpkg -i emby-server-deb_4.7.5.0_armhf.deb
```
### Commands to manage Emby
sudo systemctl status emby-server
sudo systemctl restart emby-server
sudo systemctl stop emby-server



## Install Jellyfin

```
sudo apt install curl gnupg

curl -fsSL https://repo.jellyfin.org/ubuntu/jellyfin_team.gpg.key | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/jellyfin.gpg

echo "deb [arch=$( dpkg --print-architecture )] https://repo.jellyfin.org/$( awk -F'=' '/^ID=/{ print $NF }' /etc/os-release ) $( awk -F'=' '/^VERSION_CODENAME=/{ print $NF }' /etc/os-release ) main" | sudo tee /etc/apt/sources.list.d/jellyfin.list

sudo apt-get update
sudo apt install apt-transport-https lsb-release
sudo apt install jellyfin

```

### Commands to manage Jellyfin
```
sudo systemctl status jellyfin.service
sudo systemctl restart jellyfin.service
sudo systemctl stop jellyfin.service
```

### Enable Hardware encoding
```
sudo usermod -aG video jellyfin
sudo systemctl restart jellyfin
```

#### Increase GPU Memory
```
sudo nano /boot/config.txt
```
Add/update the following parameter
```
gpu_mem=256
```

##### Verify
```
sudo reboot
vcgencmd get_mem arm
vcgencmd get_mem gpu
```

## Mount External HDD
```
sudo apt-get install ntfs-3g
```

```
df -h

sudo blkid /dev/sda1

/dev/sda1: LABEL="Media Drive" BLOCK_SIZE="512" UUID="ABCDEFGHIJK" TYPE="ntfs" PARTLABEL="My Passport" PARTUUID="********"
```

```
sudo mkdir -p /mnt/usb1
sudo chown -R jasmohan:jasmohan /mnt/usb1
```

```
sudo vi /etc/fstab
UUID=ABCDEFGHIJK /mnt/usb1 ntfs defaults,auto,users,rw,nofail,noatime 0 0
```

```
sudo umount /dev/sda1
sudo mount -a
```

```
sudo reboot
```
