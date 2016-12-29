# ath10k-QCA6174 wireless adapter firmware

## Useful links
1.https://github.com/kvalo/ath10k-firmware
2.https://wireless.wiki.kernel.org/en/users/drivers/ath10k
3.https://github.com/jeremyb31/backport-ath10k-dkms/blob/master/install-ath10k
4.https://forum.ubuntu-fr.org/viewtopic.php?id=1986604

## Firmware error (dmesg | grep -i error | grep -i ath):
[    9.604898] ath10k_pci 0000:01:00.0: Direct firmware load for ath10k/cal-pci-0000:01:00.0.bin failed with error -2
[    9.605791] ath10k_pci 0000:01:00.0: Direct firmware load for ath10k/QCA6174/hw2.1/firmware-5.bin failed with error -2
[    9.605806] ath10k_pci 0000:01:00.0: Direct firmware load for ath10k/QCA6174/hw2.1/firmware-4.bin failed with error -2
[    9.605817] ath10k_pci 0000:01:00.0: Direct firmware load for ath10k/QCA6174/hw2.1/firmware-3.bin failed with error -2
[    9.605827] ath10k_pci 0000:01:00.0: Direct firmware load for ath10k/QCA6174/hw2.1/firmware-2.bin failed with error -2
[    9.605838] ath10k_pci 0000:01:00.0: Direct firmware load for ath10k/QCA6174/hw2.1/firmware.bin failed with error -2
[   10.264853] usb 2-8: Direct firmware load for ar3k/AthrBT_0x00000200.dfu failed with error -2
[   10.264869] ath3k: probe of 2-8:1.0 failed with error -2

## Check current system
lshw -class Network
lspci | grep -i ath
dmesg | grep -i error | grep -i ath

rfkill list
iwconfig
ip link show wlp1s0

## How to install
git clone https://github.com/kvalo/ath10k-firmware $HOME/b2c/config/ath10k-firmware

sudo rm -R /lib/firmware/ath10k
sudo rmmod ath10k_pci
sudo rmmod ath10k_core

sudo cp -R $HOME/b2c/config/ath10k-firware /lib/firmware/ath10k
sudo mv /lib/firmware/ath10k/QCA6174/hw2.1/firmware-5.bin_SW_RM.1.1.1-00157-QCARMSWPZ-1 /lib/firmware/ath10k/QCA6174/hw2.1/firmware-5.bin
echo 'options ath10k_core skip_opt=Y' | sudo tee /etc/modprobe.d/ath10k_core.conf

sudo modprobe -v ath10k_core
sudo modprobe -v ath10k_pci

## How to wireless up
rfkill unblock wifi
sudo ifconfig wlp1s0 up

## How to scan existing wireless access point
sudo iwlist wlp1s0 scan
sudo iwlist wlp1s0 scan | grep -i ssid

## How to connect on existing network
sudo wpa_supplicant -B -i wlp1s0 -c /etc/wpa_supplicant_neuf_8d10.conf
sudo dhclient
sudo systemctl restart ntp.service
sudo systemctl status ntp

## How to turn off wired connection
sudo ip link set enp2s0 down
