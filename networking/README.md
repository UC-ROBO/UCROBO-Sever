# Networking

## Table of Contents

* [Home](/README.md)

#### Server Side

Run the following command within the Proxmox shell of the Node (not in any LXC Containers). Why? Should any LXC containers go down, remote connections should still work provided that the actual node and Proxmox remains operational making remote debugging possible, remember if *you* mess up the server, Ruhan is going to be mad if he has to come in.


```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

This will download the Tailscale Linux Client onto the server. But before Tailscale can be deployed, some configurations need to be made, the first being to enable IP Forwarding.

Run the following commands in order to enable IP Forwarding

```bash
echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sysctl -p
```

```bash
tailscale up --advertise-routes=[subnet range/subnet mask]
#Sample command: tailscale up --advertise-routes=192.168.1.0/24
```

#### Client Side

* **Windows**
* **Mac**
* **Linux**
  * **CLI** (Universal): `curl -fsSL https://tailscale.com/install.sh | sh`
  * **GUI** (Requires base CLI package)
    * Arch based Distros - [Trayscale](https://github.com/DeedleFake/trayscale) (Install via AUR) 
* **iOS**: Install the official Tailscale app which can be found [here](https://apps.apple.com/us/app/tailscale/id1470499037)
* **Android**: Install the official Tailscale app which can be found [here](https://play.google.com/store/apps/details?id=com.tailscale.ipn&hl=en_AU)

