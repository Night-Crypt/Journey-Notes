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

5. Extra
Waydroid_Script by casualsnek is an excellent utility 
install it from https://github.com/casualsnek/waydroid_script

```bash
git clone https://github.com/casualsnek/waydroid_script
cd waydroid_script
python3 -m venv venv
venv/bin/pip install -r requirements.txt
sudo venv/bin/python3 main.py
```
