# VM & Container setup
## Intruduction and prerequisites
VMs or containers are here to play the role of reached instances through the Apache Guacamole server. These instances need both a Wireguard setup to be securely accessible via the Apache Guacamole server and a method to connect. Here I will present two types of connections: ssh and VNC.

In this section I will cover the VM side installation for the VM for the overall infrastructure. It means that I am "only" going to cover:
- The wireguard part
- The "server" part for the Guacamole connection (ssh, VNC)

Other parts of configuration are then unlikely to be discussed here. It means than your should already have working:
- One or more VM / container - it can be any technology working under the hood
- At the moment of use, a given IP address and being able to reach the HUB of the wireguard topology

Also, I will examples on a debian-based distributions but these steps can basically be directly transposed to others using their own package manager and configuration tools.

**Note, I did not tried with a Windows VM yet. Maybe it will arrive one day.

## Installation of wireguard
**This part is identical on both spokes**
Here there is not that much to achieve since we are only going to install Wireguard and generate keys. First, download the wireguard package as given on the [wireguard website](https://www.wireguard.com/install/): 
```bash
apt install wireguard
```

Then generate both public and private keys:
```bash
# as root
wg genkey > host.pub
wg pubkey < host.pub > host.priv
```

## Wireguard configuration
Here we are going to use the wg-quick tool to make to process easy. Create the file `/etc/wireguard/wg0.conf` with the following content:
```bash
[Interface]
Name=wg0
Address=192.168.2.49/32
PrivateKey=<private key you generated earlier>

[Peer] # Section for the server
AllowedIPs=192.168.2.1/24 # Can be more granular or more general according to your use case
PublicKey=<PublicKey of your Hub>
PersistentKeepalive = 25 
```

Note that you can replace wg0 by any name you wish for your interface. To this end, you need to change the filename and the Name field of the configuration file

Also note that the `PersistentKeepalive` parameter is important so the connection between the Hub and other spokes can happen all the time. Since the guacamole gateway's wireguard's config does not know anything about whats behind the Hub, we have to keep the link up and running. Else, once the connection is finished and because wiregaurd keeps the tunnel "silent" by default we would not be able to reconnect to this spoke again. 

That done, you should be able to enable and run the service: 
```bash
systemctl enable --now wg-quick@wg0.service
```

## Installation of the VNC server (for the GUI Spoke)
For the sake of this tutorial I will use TigerVNC but you can use whatever you want.

First install packages from your package manager
```
apt install tigervnc-standalone-server tigervnc-viewer
touch ~/.vnc
```

Create a vncuser with `adduser`
Create a password for vnc.
```bash
su - vncuser
vncpassword
```
Create the `~/.vnc/xstartup` file (for GNOME): 
```txt
#!/bin/sh
xrdb $HOME/.Xresources
vncconfig -iconic &
dbus-launch --exit-with-session gnome-session &
```

Set right permissions: 
```
sudo chmod u+x  ~/.vnc/xstartup 
sudo chmod 777 ~/.vnc/xstartup
```

Start the service
```bash
systemctl enable vncserver@:1
```

The server should be up and running, you could check with a classical agent such as KRDC (KDE) or Connections (GNOME).

## Installation of the ssh server 
I do not think I have to explain how to install ssh on the container but just in case because most distributions ship openssh server by default but you never know:
```bash
apt install openssh-server
```

Then you can edit the file `/etc/ssh/sshd_config` to you needs. I would recommend at least:
- `PermitRootLogin no`
- `PubkeyAuthentication yes`
- `PasswordAuthentication no`
- Later on you may want to let ssh only accessible via the wireguard interface or some specific IPs 

Finally restart the service and enable it if that's not already the case. 
```
systemctl restart sshd
systemctl enable --now sshd
```
For a lot of VMs ssh is enabled by default. Ensure that at this step you are able to connect to the ssh server from another computer.

