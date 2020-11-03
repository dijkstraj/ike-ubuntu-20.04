# IKE patch for Ubuntu 20.04

When following [these instructions](https://sites.google.com/view/uptolinux) I ran into problems with the different line endings used in the source code.
I've split the patch into two: one with Windows line endings (CRLF) and one with Unix line endings (LF).
Run the following commands to apply the patch:

```bash
curl -s https://raw.githubusercontent.com/dijkstraj/ike-ubuntu-20.04/master/ike-lf.patch | patch -p1
curl -s https://raw.githubusercontent.com/dijkstraj/ike-ubuntu-20.04/master/ike-crlf.patch | patch -p1 --binary
```

So the full script for building and installing the VPN client would be:

```bash
curl -s https://raw.githubusercontent.com/dijkstraj/ike-ubuntu-20.04/master/ike-crlf.patch | patch -p1 --binary
cd /tmp
tar xvf ike-2.2.1-release.tgz
cd /tmp/ike
curl -s https://raw.githubusercontent.com/dijkstraj/ike-ubuntu-20.04/master/ike-lf.patch | patch -p1
curl -s https://raw.githubusercontent.com/dijkstraj/ike-ubuntu-20.04/master/ike-crlf.patch | patch -p1 --binary
sudo apt update && sudo apt install build-essential libssl-dev libaudio-dev libcups2-dev  cmake libedit-dev g++
cmake -DCMAKE_INSTALL_PREFIX=/usr -DQTGUI=NO -DETCDIR=/etc -DNATT=YES
make
sudo make install
```

