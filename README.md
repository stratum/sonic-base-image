# sonic-base-image

This repository is a collection of images and other build artifacts of SONiC for use as the base OS in [Stratum](https://github.com/stratum/stratum).
Check the [Release](https://github.com/stratum/sonic-base-image/releases) page for download links.


## Building a kernel header tar

SONiC delivers the kernel header sources in two Debian packages. This format makes consumption harder, e.g., when building kernel modules.
We merge those two packages into one tarball with these steps:

```bash
docker run -it -v $(pwd):$(pwd) -w $(pwd) debian:10 bash
# Inside the container
apt update
apt install xz-utils
apt-get install -y --no-install-recommends ./linux-headers-4.19.0-12-2-common.deb
apt-get install -y --no-install-recommends ./linux-headers-4.19.0-12-2-amd64.deb
mkdir -p /usr/src/linux-headers-4.19.0-12-2-merged
rsync -ahPL /usr/src/linux-headers-4.19.0-12-2-amd64/ /usr/src/linux-headers-4.19.0-12-2-merged
rsync -ahPL /usr/src/linux-headers-4.19.0-12-2-common/ /usr/src/linux-headers-4.19.0-12-2-merged
cd  /usr/src/
tar cJf linux-headers-4.19.0-12-2-merged.tar.xz linux-headers-4.19.0-12-2-merged
cp linux-headers-4.19.0-12-2-merged.tar.xz /path/to/outside/container
```
