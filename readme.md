## Allow Ip Forwarding First



* Install wireguard using `sudo apt install wireguard`
* then generate public and private key by running `umask 077 && wg genkey | tee privatekey | wg pubkey > publickey`

## ALLOW TRAFFIC FORWARDNING

* open this file `sudo nano /etc/sysctl.conf`
* find this line `net.ipv4.ip_forward=1` and uncomment it
* find this line `net.ipv6.ip_forward=1` and uncomment it.

## SERVER CONFIG

. Create a `wg0.conf` file at `/etc/wireguard`
. Run `sudo nano /etc/wireguard/wg0.conf` as paste the below config as it is
```
[Interface]
PrivateKey = wiregurad-private-key (generate using umask 077 && wg genkey > privatekey)
Address = 10.0.0.1/24
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 51820

[Peer]
PublicKey = 3NAdO8Z0PtF850IP7CiwnhrInE3SjyBCcoSFaSVA+R0=
AllowedIPs = 10.0.0.2/32

[Peer]
PublicKey = C9RtuVI6HHGDEuX/LwnlxRmmwefzOfl70/iubHO31hA=
AllowedIPs = 10.0.0.3/32
```

Here above `eth0` is going to be the default internet of the server
to check the default run `ip route list default`
