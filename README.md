# debian-firecracker

Docker container to build a linux kernel and ext4 rootfs compatible with [firecracker](https://github.com/firecracker-microvm/firecracker).

## Usage
Build the container:

```
docker build -t debian-firecracker .
```

Build the image:

```
docker run --privileged -it --rm -v $(pwd)/output:/output debian-firecracker
```

Start the image with firectl

```
# copy image and kernel
cp output/vmlinux debian-vmlinux
cp output/image.ext4 debian.ext4

# resize image
truncate -s 5G debian.ext4
resize2fs debian.ext4

#launch firecracker
firectl --kernel=debian-vmlinux --root-drive=debian.ext4 --kernel-opts="init=/bin/systemd noapic reboot=k panic=1 pci=off nomodule console=ttyS0"
```
