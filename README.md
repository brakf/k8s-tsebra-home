## PI Setup
- Install Raspberry Pi OS (Full; not lite!) on SD card via Raspberry Pi Imager (configure hostname, wifi, locale, timezone, password, enable SSH)

- Configure m.2 ssd hat: https://www.raspberrypi.com/documentation/accessories/m2-hat-plus.html#m2-hat-plus

- Install Raspberry Pi OS on m.2 ssd hat
   - was a real pain... need full version of Raspberry Pi OS to run raspi-imager
   - get image url and sha256 from: https://www.raspberrypi.org/software/operating-systems/
   - get device name with: `lsblk`
   - write image:
   ```
   sudo rpi-imager --cli https://downloads.raspberrypi.com/raspios_lite_arm64/images/raspios_lite_arm64-2024-11-19/2024-11-19-raspios-bookworm-arm64-lite.img.xz /dev/nvme0n1 --disable-verify
   ``` 
   (--sha for verification didn't work for me... so i just went with the --disable-verify)

   - check new partitions: `sudo parted -l` and o. find boot partition (the one with FAT formationg)
   - mount boot partition: `sudo mount /dev/nvme0n1p1 /mnt`
   create empty file ssh to activate ssh:
   touch /mnt/ssh

   - vim /mnt/wpa_supplicant.conf:
   ```
country=de
update_config=1
ctrl_interface=/var/run/wpa_supplicant
network={ ssid="FRITZ!Box 7490-5GHz" psk="97209255103289181794"}
   ```

   change boot order to boot from nvme

get ip from router and connect via ssh 

ssh pi@192.168.178.66




###fuck all of this
1. install os using raspi-imager on mac on two devices; configure one properly;
use the other one to dd the image to the devices

sudo dd if=/dev/sda of=/dev/nvme0n1

- install k3s etc...




1. Install ArgoCD:

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

create ingress:

```
kubectl apply -f argocd/ingress.yaml
```



2. Bootstrap cluster configuration (e.g. longhorn as storage provider):

```   
kubectl apply -f bootstrap-cluster.yaml
```

3. Bootstrap applications:

```
kubectl apply -f bootstrap-applications.yaml
```
kubectl apply -f bootstrap-cluster.yaml