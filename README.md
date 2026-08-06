# My Fedora postinstall guide

### 1) Set dnf flags:

In this file:
```
/etc/dnf/dnf.conf
```
Specify options based on your preferences:

```
# see `man dnf.conf` for defaults and possible options
k3b
# Default config options:
[main]
max_parallel_downloads=5 
defaultyes=True          
```
Explanation:
- `max_parallel_downloads=5`  change amount of parallel donwloads to 5 
- `defaultyes=True`          make "Yes" default answer for dnf actions 

### 2) Install rpmfusion repos:

free:
```
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```
nonfree:
```
sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### 3) Perform full system upgrade:

```
sudo dnf upgrade --refresh
```

### 4) Nvidia drivers:

```
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs.{i686,x86_64} libva-nvidia-driver.{i686,x86_64} xorg-x11-drv-nvidia-cuda

# For latest drivers (555-570) + wayland you might want to also disable GSP Firmware
# source: https://forums.developer.nvidia.com/t/major-kde-plasma-desktop-frameskip-lag-issues-on-driver-555/293606
sudo grubby --update-kernel=ALL --args=nvidia.NVreg_EnableGpuFirmware=0
```

### 5) Disable unneeded services on startup:
Discover update notifications, waiting for network and others:
```
sudo systemctl disable NetworkManager-wait-online.service
sudo rm /etc/xdg/autostart/org.kde.discover.notifier.desktop /etc/xdg/autostart/vmware-user.desktop /etc/xdg/autostart/vboxclient.desktop /etc/xdg/autostart/spice-vdagent.desktop
```

### 6) Install codecs:
More info here: https://rpmfusion.org/Howto/Multimedia
```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf install -y libheif-freeworld qt-heif-image-plugin
sudo dnf install @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

### 7) Remove preinstalled applications
Minimize KDE session:
```
sudo dnf group remove libreoffice && sudo dnf remove libreoffice-core \
 kmahjongg kmines kpat \
 akregator kmail headerthemeeditor ktn neochat pimdataexporter sieveeditor kmousetool kmouth im-chooser korganizer kaddressbook khelpcenter \
 dragon elisa-player kamoso kolourpaint skanpage k3b gcdmaster qrca ktorrent kdeconnect nwg-panel mediawriter krusader digikam showfoto uuctl \
 kleopatra kcharselect kde-connect plasma-welcome kdebugsettings kjournald gnome-abrt kfind 
 
```
