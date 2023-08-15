# CreateYourOwnWindowsImage
Create Your Own Windows Image for DigitalOcean

## STEP 1 - Creating Windows Image

### Install Qemu & Additional package
```
apt-get update && apt-get install qemu -y
apt install qemu-utils -y && apt install qemu-system-x86 -y
```

### Create disk image
```
qemu-img create -f raw window.img 16G
```

### Download Drive 
```
wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso
```

### Download windows ISO   
```
wget -O windows.iso 'https://linkwindowsiso'
```

### Run Virtualization Windows Install
```
qemu-system-x86_64 \
	-m 3G \
	-cpu host \
	-enable-kvm \
	-boot order=d \
	-drive file=windows.iso,media=cdrom \
	-drive file=windows.img,format=raw,if=virtio \
	-drive file=virtio-win.iso,media=cdrom \
	-vnc :0 \
```

## STEP 2 - Upload Image to Sourceforge
### Install sshfs & login to your sourceforge account via sshfs
```
apt install sshfs
mkdir /mnt/temp
sudo sshfs user@frs.sourceforge.net:/home/frs/project/user /mnt/temp
```

### Upload your image to Sourceforge
```
dd if=windows.img | gzip -1 | dd of=/mnt/temp/wedus.gz
```

## STEP 3 - Install windows img to your server

```
wget --no-check-certificate -O- https://master.dl.sourceforge.net/project/user/wedus.gz?viasf=1 | gunzip | dd of=/dev/vda bs=3M status=progress
```

## Additional - command windows for change IP address Ethernet
```
netsh -c interface ip set address name="Ethernet Instance 0" source=static address=YourIP mask=YourMask gateway=YourGateway
```
