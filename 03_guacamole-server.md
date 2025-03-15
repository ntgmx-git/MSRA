## Introduction
The installation of the Apache Guacamole server is both containing a bit of steps (but not difficult ones) and well explained on the official website: https://guacamole.apache.org/doc/gug/introduction.html. 
Thereby, I will just add either points that could be usefull or the configuration part related to this project.

## Wireguard configuration
The configuration for wireguard is indeed similar to all other peers (not the router), since its aim is just to be able to reach the Wireguard Hub to reach other peers. Therefore you just need to replay instructions to generate keys and setup the daemon. Also, think about using the `PersistentKeepalive` option. 

## Adding connections to Apache Guacamole
Here, we'll just add the wireguard IPs with credentiels / keys to Guacamole connections. Then:
1. In protocol field choose either SSH or VNC (in this project)
2. Under PARAMETERS / Network, give the corresponding wireguard peer's address in `Hostname` field and the corresponding port (~22 for SSH and likely around 5901 for VNC).
3. Under PARAMETERS / Authentication, input `Username`, `Password` or `Private Key` and its `Passphrase`. Here, think that you need to enter your `vncuser` login password in case of a VNC connection.

And well, thats all what you need for a basic setup. You can take a look at other options if you want to.



## Adding 2FA 
I tested adding [2FA for Guacamole authentication](https://guacamole.apache.org/doc/gug/totp-auth.html) via TOTP. It worked well and I can only recommend implementing it. 

## Notes
- The first time you want to establish a connection, think about ping the wireguard HUB and maybe the VM on its Wireguard address. It seems to ensure the connection is established and kept with the `PersistentKeepalive`. The first time I was not able to connect to the VM and it was alright after ping from the server and from the VM, both to the Wireguard HUB.
- At the time of writing of this page, the guacamole server only support up to tomcat9 (and above versions).

