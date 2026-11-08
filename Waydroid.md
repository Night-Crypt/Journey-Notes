Waydroid is an android container the "best way" to run android apps on linux 
-  U need to use a wayland compositor as waydroid doesnt support x11
	- (eg. KDE, Gnome, Niri, Hyprland etc.)


1. Install Waydroid from Package manager
``` bash
sudo dnf install waydroid
```

2. Initialize
```bash
sudo waydroid init -c https://ota.waydro.id/system -v https://ota.waydro.id/vendor
sudo waydroid init -s GAPPS -f -c https://ota.waydro.id/system -v https://ota.waydro.id/vendor # if gapps is required
```

2-1 
 if download is slow go to sourceforge and install the latest vendor and system files 
	 (if the download is still slow make sure to change ur mirror from "Problem downloading")
```bash
cd /path/to/download
unzip *.zip
ls -lh *.img # the files should show up as vendor.img and system.img
sudo mkdir -p /etc/waydroid-extra/images
sudo mv system.img vendor.img /etc/waydroid-extra/images/ # make relevant changes here if the names r different

## Initialize now
sudo waydroid init -f # should not download anything

## Check 
waydroid status
```

3. Set Network and firewalls
```bash
sudo systemctl enable --now waydroid-container

sudo firewall-cmd --zone=trusted --add-interface=waydroid0 --permanent
sudo firewall-cmd --zone=trusted --add-port=53/udp --add-port=67/udp --permanent
sudo firewall-cmd --zone=trusted --add-forward --permanent
sudo firewall-cmd --reload
```

4. Boot
``` bash
waydroid session start
waydroid show-full-ui # or launch from dmenu/application launcher
```

4-1. 
FOR GAPPS
	u need to certify and "tell" google that its a "real" device
		thankfully the overall process is very easy 
```bash
sudo waydroid shell
ANDROID_RUNTIME_ROOT=/apex/com.android.runtime ANDROID_DATA=/data ANDROID_TZDATA_ROOT=/apex/com.android.tzdata ANDROID_I18N_ROOT=/apex/com.android.i18n sqlite3 /data/data/com.google.android.gsf/databases/gservices.db "select * from main where name = \"android_id\";"

# copy the long number it returns

```
paste it here[https://google.com/android/uncertified](https://google.com/android/uncertified) 
```bash
# then restart
waydroid session stop
waydroid session start
waydroid show-full-ui
```

6. Extra
Waydroid_Script by casualsnek is an excellent utility 
install it from https://github.com/casualsnek/waydroid_script

```bash
git clone https://github.com/casualsnek/waydroid_script
cd waydroid_script
python3 -m venv venv
venv/bin/pip install -r requirements.txt
sudo venv/bin/python3 main.py
```
- Installs GApps (OpenGApps or MindTheGapps), Magisk, and microG
- Installs ARM translation (libhoudini or libndk) for ARM-only apps 
	- Highly recommend u do this ^
- Installs Widevine (for DRM video), SmartDock, and a few other extras