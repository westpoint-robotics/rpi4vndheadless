# rpi4vndheadless
How to convert RPi4 to headless mode with VNC viewer

### 0. Configuration

- RPi4
- Ubuntu 22.04
  
### 1. RealVNC installation

```http
 https://www.realvnc.com/en/connect/download/vnc/
```


Get VNC-Server Raspberry Pi / arm64 and download
Install by running the following command

```http
 sudo dpkg -i VNC-Server-<version>-Linux-ARM64.deb
```
### 2. If error occurs during the installation of RealVNC

Error
```http
Selecting previously unselected package realvnc-vnc-server.
(Reading database ... 188247 files and directories currently installed.)
Preparing to unpack VNC-Server-6.9.1-Linux-ARM64.deb ...
Unpacking realvnc-vnc-server (6.9.1.46706) ...
Setting up realvnc-vnc-server (6.9.1.46706) ...
Updating /etc/pam.d/vncserver
Updating /etc/pam.conf... done

NOTICE: common configuration in /etc/pam.d contains the following modules:
   pam_sss.so
The default vncserver PAM configuration only enables pam_unix. See
`man vncinitconfig' for details on any manual configuration required.

Looking for font path... not found.
/usr/bin/vncserver-x11: error while loading shared libraries: libbcm_host.so.0: cannot open shared object file: No such file or directory
/usr/bin/vncserver-x11: error while loading shared libraries: libbcm_host.so.0: cannot open shared object file: No such file or directory
Installed systemd unit for VNC Server in Service Mode daemon
Start or stop the service with:
  systemctl (start|stop) vncserver-x11-serviced.service
Mark or unmark the service to be started at boot time with:
  systemctl (enable|disable) vncserver-x11-serviced.service

Installed systemd unit for VNC Server in Virtual Mode daemon
Start or stop the service with:
  systemctl (start|stop) vncserver-virtuald.service
Mark or unmark the service to be started at boot time with:
  systemctl (enable|disable) vncserver-virtuald.service

Processing triggers for mailcap (3.70+nmu1ubuntu1) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu3) ...
Processing triggers for desktop-file-utils (0.26-1ubuntu3) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for shared-mime-info (2.1-2) ...
```
#### How to solve this problem?
```http
cd /usr/lib/aarch64-linux-gnu/

sudo ln libvcos.so /usr/lib/libvcos.so.0
sudo ln libvchiq_arm.so /usr/lib/libvchiq_arm.so.0
sudo ln libbcm_host.so /usr/lib/libbcm_host.so.0
```
```http
sudo nano /etc/gdm3/custom.conf
WaylandEnable=false # uncomment this line.
```
```http
sudo systemctl enable vncserver-virtuald.service
sudo systemctl enable vncserver-x11-serviced.service
sudo systemctl start vncserver-virtuald.service
sudo systemctl start vncserver-x11-serviced.service

sudo reboot
```
### 3. To make the RPi4 headless -> create dummy display

```http
sudo apt update
sudo apt install xserver-xorg-video-dummy
sudo cp /etc/X11/vncserver-virtual-dummy.conf /etc/X11/xorg.conf
sudo reboot
```

### 4. If you want to un-do the headless RPi4

```http
sudo mv /etc/X11/xorg.conf /etc/X11/xorg.conf.dummy
sudo reboot
```

### 5. Final

Attempt access from the master PC which has RealVNC Viewer to Headless RPi4 with RealVNC Server
