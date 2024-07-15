# My Fedora postinstall guide

### 1) Set dnf flags:

In this file:
```
/etc/dnf/dnf.conf
```
Specify options based on your preferences:

```
# see `man dnf.conf` for defaults and possible options

# Default config options:
[main]
gpgcheck=True
installonly_limit=3
clean_requirements_on_remove=True
best=False
skip_if_unavailable=True
# Optional changes:
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

### 4) Disable unneeded services on startup:
```
sudo systemctl disable NetworkManager-wait-online.service
sudo rm /etc/xdg/autostart/org.gnome.Software.desktop
```

### 5) Install codecs:

```
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf update @sound-and-video
```

### 6) Nvidia hardware acceleration codecs:

```
sudo dnf install libva-nvidia-driver.{i686,x86_64}
```
