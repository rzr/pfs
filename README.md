pfs
===

pCloud filesystem client

To compile, you need fuse and the openssl headers. In debian,
they're in libssl-dev and libfuse-dev, in fedora in fuse-devel and
openssl-devel.

## Setup instructions on `systemd` based machines

### Install pfs

```sh
(sudo) yum install fuse-devel openssl-devel
git clone https://github.com/pcloudfs/pfs.git
cd pfs
make
(sudo) make install
```

### Get auth token

```sh 
curl https://api.pcloud.com/userinfo?getauth=1&username=<email>&password=<password>
```

And keep the auth bit.

### Autostart

Create a systemd service

```sh
gedit /usr/lib/systemd/system/pfs.service
```

And paste in:

```
[Unit]
Description=pCloud mount

[Service]
Type=oneshot
User=<your user>
Group=<your user>
RemainAfterExit=yes
ExecStart=/usr/bin/mount.pfs --auth <you auth token here> /run/media/<your user>/pCloud
ExecStop=/usr/bin/umount /run/media/<your user>/pCloud

[Install]
WantedBy=multi-user.target
```

Activate it via:

```sh
(sudo) systemctl enable pfs.service
```
