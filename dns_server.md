# Unbound DNS Setup on DietPi

## 1. Install Unbound

Run: dietpi-software

* Select Unbound (ID: 182)
* Proceed with installation

---

## 2. Restart and Enable dnsmasq

Run the following commands:

sudo systemctl restart dnsmasq
sudo systemctl enable dnsmasq
sudo reboot

---

## 3. Enable and Start Unbound

Run:

systemctl status unbound
systemctl enable unbound
systemctl start unbound

---

## 4. Configure Unbound

Edit the configuration file:

nano /etc/unbound/unbound.conf.d/dietpi.conf

Add or update the following:

server:
    interface: 0.0.0.0
    port: 53
    access-control: 192.168.0.0/24 allow
    do-ip4: yes
    do-udp: yes
    do-tcp: yes
    cache-min-ttl: 3600
    cache-max-ttl: 86400
    prefetch: yes
    hide-identity: yes
    hide-version: yes

forward-zone:
    name: "."
    forward-addr: 1.1.1.1
    forward-addr: 8.8.8.8

---

## 5. Restart Unbound

Run:

systemctl restart unbound

---

## 6. Verify Service

Run:

systemctl status unbound --no-pager

Expected result:
Active: active (running)

---

## 7. Test DNS Resolution

Run:

nslookup google.com 192.168.0.101

Example output:

Server:  UnKnown
Address:  192.168.0.101

Name:    google.com
Addresses:  2a00:1450:4019:815::200e
142.250.202.142

---

## Notes

* Replace 192.168.0.101 with your server IP
* Ensure port 53 is not blocked or already in use
* To check logs:

journalctl -u unbound

---