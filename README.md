update-docker-dns
=================

Selectively expose docker containers by name.

[Install 1/3] Copy the script to where it belongs
-------------------------------------------------

```
sudo cp update-docker-dns.sh /usr/loca/bin
```

[Install 2/3] Add dnsmasq support
---------------------------------

* If you **have** a Network Manager (most linux desktops), and it is
using *dnsmasq*:

```
sudo bash -c "cat > /etc/NetworkManager/dnsmasq.d"
addn-hosts=/etc/docker-container-hosts
^D
```

```
sudo systemctl restart network-manager.service
```

* If you **don't** have a Network Manager, or it is not using *dnsmasq*:

```
sudo apt install -y dnsmasq
sudo bash -c "cat > /etc/dnsmasq.d/docker-dns"
addn-hosts=/etc/docker-container-hosts
^D
```

```
sudo systemctl restart dnsmasq.service
```

[Install 3/3] Register it as a system service
---------------------------------------------

```
sudo bash -c "cat > /etc/systemd/system/docker-dnsmasq.service"
[Unit]
Description=Update dnsmasq with selected docker containers
After=docker.service dnsmasq.service network-manager.service

[Service]
ExecStart=/usr/local/bin/update-docker-dns.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
^D
```

```
sudo systemctl enable docker-dnsmasq.service
sudo systemctl start docker-dnsmasq.service
```

Usage
-----
See [Install and test example](https://www.theimpossiblecode.com/blog/docker-expose-service/).
See [Usage example](https://www.theimpossiblecode.com/blog/docker-wordpress-multisite-with-subdomains/).
