# Minishift Ubuntu Installation Guide
## Download Minishift
```
sudo apt update
wget https://github.com/minishift/minishift/releases/download/v1.11.0/minishift-1.11.0-linux-amd64.tgz
```
## Extract Minishift
```
tar -xvf minishift-1.11.0-linux-amd64.tgz
cd minishift-1.11.0-linux-amd64/
```

## Install KVM Drivers
```
sudo curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.7.0/docker-machine-driver-kvm -o /usr/local/bin/docker-machine-driver-kvm
sudo chmod +x /usr/local/bin/docker-machine-driver-kvm
sudo apt install libvirt-bin qemu-kvm
sudo usermod -a -G libvirtd $USER
newgrp libvirtd
```

## Start Minishift
```
./minishift start
```
