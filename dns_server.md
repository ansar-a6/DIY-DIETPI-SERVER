# Unbound DNS Setup on DietPi

## 1. Router Settings
Before starting, ensure your server has a static IP address.
1.  Access your router's administration page.
2.  Assign a **Static IP address** to your server using its **MAC address** (Physical address).
3.  Set your router's primary DNS server to this static IP.
4.  Set a secondary DNS server (e.g., `1.1.1.1` or `8.8.8.8`) as a backup. This ensures your household maintains internet access if the local server is temporarily offline.

## 2. Install Unbound
Run the DietPi software tool:
1.  Execute: `dietpi-software`
2.  Select **Unbound** (ID: 182).
3.  Proceed with the installation.

## 3. Enable and Restart dnsmasq
To ensure `dnsmasq` works alongside Unbound, run the following commands:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
sudo systemctl enable dnsmasq
sudo systemctl restart dnsmasq
</pre>

## 4. Enable and Start Unbound
Ensure the Unbound service is active and set to start on boot:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
sudo systemctl enable unbound
sudo systemctl start unbound
</pre>

## 5. Configure Unbound
Edit the configuration file to define your local network and upstream DNS providers:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
sudo nano /etc/unbound/unbound.conf.d/dietpi.conf
</pre>

Add or update the content with the following configuration:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
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
</pre>

**Explanation:**
*   **server section:** Defines how Unbound listens for requests. It is configured to allow traffic from your local subnet (`192.168.0.0/24`) and includes caching settings (TTL) to improve performance.
*   **forward-zone section:** Specifies where Unbound should look if it doesn't have the answer in its cache. Here, it forwards requests to public upstream servers (`1.1.1.1` and `8.8.8.8`).

## 6. Restart Unbound
Apply the changes by restarting the service:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
sudo systemctl restart unbound
</pre>

## 7. Verify Service
Check if Unbound is running correctly:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
systemctl status unbound --no-pager
</pre>

**Expected result:**
<pre style="color:#00ff00; background:#000000; padding: 10px;">
Active: active (running)
</pre>

## 8. Test DNS Resolution
Run the following command from a client device (laptop/PC) to test the server:

<pre style="color:#00ff00; background:#000000; padding: 10px;">
nslookup google.com 192.168.0.101
</pre>

**Example output:**
<pre style="color:#00ff00; background:#000000; padding: 10px;">
Server:  UnKnown
Address:  192.168.0.101

Name:    google.com
Addresses:  2a00:1450:4019:815::200e
142.250.202.142
</pre>
*Note: The first lookup might be slightly slow. Subsequent lookups for the same domain will be much faster as they are served from the local cache.*

## Notes
*   **IP Address:** Replace `192.168.0.101` with your actual server IP address.
*   **Ports:** Ensure port `53` is not blocked by a firewall or used by another service.
*   **Logs:** To troubleshoot, view the service logs:
    `journalctl -u unbound`

## Summary
You have successfully installed and configured `unbound.service` as your local DNS resolver, improving both privacy and speed for your network.
