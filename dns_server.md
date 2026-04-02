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

## FAQ: Why use a Local DNS Server?

### 1. What is the benefit of a DNS server?
By hosting your own DNS server like Unbound, you gain several advantages:
*   **Privacy:** Your ISP (Internet Service Provider) can no longer easily track every website you visit.
*   **Speed:** Frequently visited sites are stored in a local "cache," allowing your devices to get the IP address instantly without asking the internet.
*   **Reliability:** You are no longer dependent on your ISP's often slow or unstable DNS servers.

### 2. Does my router really need this? (Modern vs. Tenda N301)
*   **Modern Routers:** While modern high-end routers have faster processors and more RAM, they still lack the advanced caching and privacy features of a dedicated Unbound server.
*   **Your Tenda N301:** This is a budget-friendly, entry-level router with very limited CPU and RAM. When multiple devices (phones, TVs, laptops) all ask the router for DNS at once, it can "choke" or slow down. By offloading this task to your DietPi server, you're literally giving your router a "break," allowing it to focus purely on moving data packets, which can improve your overall network stability.

### 3. What does it actually do?
Think of a DNS server as the **"Phonebook of the Internet."** 
Computers don't understand names like `google.com`; they only understand numbers called IP addresses (like `142.250.202.142`). When you type a URL, your local DNS server quickly looks up the "phone number" (IP) for that "name" (URL). Because it's now sitting right next to you on your local network, that lookup happens almost instantly.

### 4. How much does it benefit Latency, Speed, and Connections?
*   **Latency:** This is where you see the biggest win. A normal DNS lookup to your ISP might take **20ms to 100ms**. A cached lookup from your local Unbound server takes **less than 1ms**. This makes the web feel "snappier" because pages start loading the moment you hit Enter.
*   **Speed:** It won't increase your raw "Megabits per second" download speed, but it reduces the "waiting time" *before* a download starts.
*   **Connections:** It improves "Connection Stability." Budget routers like the N301 often drop connections if they get overwhelmed by too many simultaneous DNS requests. A dedicated server handles thousands of requests effortlessly, ensuring your connection stays solid even when many people are using the internet.
