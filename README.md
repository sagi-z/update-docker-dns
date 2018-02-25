update-docker-dns
=================

```
sudo cp update-docker-dns.sh /usr/loca/bin
```

Add dnsmasq support
-------------------

```
sudo bash -c "cat > /etc/dnsmasq.d/docker-dns"
addn-hosts=/etc/docker-container-hosts
#interface=docker0
^D
```

```
sudo systemctl restart dnsmasq.service
```

Register it as a system service
-------------------------------

```
sudo vim /etc/systemd/system/docker-dnsmasq.service
[Unit]
Description=Update dnsmasq with selected docker containers
After=docker.service dnsmasq.service

[Service]
ExecStart=/usr/local/bin/update-docker-dns.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl enable docker-dnsmasq.service
sudo systemctl start docker-dnsmasq.service
```

