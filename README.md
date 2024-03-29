![Go](https://github.com/ForAllSecure/rootfs_builder/workflows/Go/badge.svg?branch=master)

Rootfs Builder
======

Rootfs builder pulls an image from a Docker registry and extracts the
rootfs.  This is equivalent to the command:

`mkdir rootfs && docker export $(docker create busybox) | tar -C rootfs -xvf -`

The rootfs generated is OCI compliant and can be run with RunC.  The
user can specify the user to chown the files to and whether or not to
use a subuid mapping in case they want to unshare user namespaces.

Installation
=====
Install Go 1.12

On debian:sid
`apt-get install -y golang-1.12-go`.

From source:
```
sudo apt-get update
wget https://dl.google.com/go/go1.12.7.linux-amd64.tar.gz
sudo tar -xvf go1.12.7.linux-amd64.tar.gz
sudo mv go /usr/local
sudo mv /usr/local/go/bin/go /bin
```

Rootfs builder can be statically built.  This statically compiles
rootfs builder in a container:

`make static`

Or if you want to develop Rootfs Builder in a container, run:
`make dev`

Usage
=====
Rootfs builder can be run with:
`./rootfs_builder <config.json>`

An example config.json looks like:
```
{
    "Name": "debian:buster",
    "Cert": "/workdir/cert",
    "Retries": 3,
    "Spec":
        {
            "Dest": "/tmp/rootfs",
            "User": "fas",
            "UseSubuid": True
        }
}
```
* **`Name`** (string, REQUIRED) Name of image to pull.
* **`Cert`** (string, OPTIONAL) Path to cert to add to root CAs for the registry.
* **`Retries`** (int, OPTIONAL) Number of attempts to connect to registry.
* **`Spec`** (dict, OPTIONAL) Spec for the rootfs.
* **`Dest`** (string, OPTIONAL) Destination to extract rootfs to.
* **`User`** (string, OPTIONAL) User to chown files to.
* **`UseSubuid`** (bool, OPTIONAL) Look up subuid mapping for giving user and chown to that uid.

Tests
=====
To run integration tests, run `make test`.

Credits
=====

This code is from [ForAllSecure](https://forallsecure.com) labs. It is
not an official ForAllSecure maintained product or offering.

Some code recycled from Google's Kaniko.

Rootfs builder can be run with:
`./rootfs_builder example_configs/code.json`

Tar intro

tar -czv --numeric-owner -f /rootfs_builder/images/alpine-minirootfs-gitcode-arm64.tar.gz .


1 get name from docker registry to file example_configs/code.json

2 make local_static

3 make dev 

rm -rf /tmp/code

4 ./rootfs_builder example_configs/nimble.json

5 cd /tmp/code
    rmcd /code

6 `tar -czv --numeric-owner -f /rootfs_builder/images/centos9-rootfs-arm64.tar.gz .`

`apt-get update
apt-get install xz`

create .tar.xz:
`tar -cJv --numeric-owner -f /rootfs_builder/images/centos9-rootfs-arm64.tar.xz .`


7 download from /images

8 add to repository https://github.com/drunkod/Anlinux-Resources/tree/master/Rootfs/CentOS/arm64


