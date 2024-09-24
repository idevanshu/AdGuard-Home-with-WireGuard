
<h1>AdGuard Home with WireGuard in Docker - Easy Setup Guide</h1>

<h3>Overview</h3>
<strong>This guide will walk you through setting up AdGuard Home, a powerful network-wide ad blocker, alongside WireGuard for a secure VPN, all in Docker. AdGuard Home helps block ads and trackers at the DNS level, while WireGuard provides a private network for secure access.</strong>

<h3>Use Case</h3>
<strong>By combining AdGuard Home and WireGuard, you can:
- Block ads and trackers across all devices on your network.
- Secure your connection while browsing through a VPN.
- Access your home network remotely using WireGuard.
- Have a single Docker environment for both services, making it easy to manage and deploy.</strong>

<h3>Setup Guide</h3>

<strong>1. Prerequisites</strong>
<ul>
    <li><strong>Docker and Docker Compose installed on your system.</strong></li>
    <pre><code>sudo apt update
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $(whoami)
exit</code> </pre>
    <li><strong>You can deploy this setup on any cloud provider offering a Linux environment.</strong></li>
    <li><strong>A domain name (optional, if you are using your home network).</strong></li>
</ul>
<strong>2. Create a Docker Compose File</strong>
<br>
<strong>1) Setup up for Wireguard (using Wireguard easy docker)</strong>
<strong>In your project directory, create a <code>docker-compose.yml</code> file with the following configuration:</strong><be>
<pre><code>docker run --detach \
  --name wg-easy \
  --env LANG=en \
  --env WG_HOST=<🚨YOUR_SERVER_IP> \
  --env PASSWORD_HASH='' \
  --env PORT=51821 \
  --env WG_PORT=51820 \
  --volume ~/.wg-easy:/etc/wireguard \
  --publish 51820:51820/udp \
  --publish 51821:51821/tcp \
  --cap-add NET_ADMIN \
  --cap-add SYS_MODULE \
  --sysctl 'net.ipv4.conf.all.src_valid_mark=1' \
  --sysctl 'net.ipv4.ip_forward=1' \
  --restart unless-stopped \
  ghcr.io/wg-easy/wg-easy</code></pre>
<strong>Now, you can open the browser and navigate to <code> your_devic_ip:51821 </code>⁠ to manage your wireguard VPN</strong>
<br>

<strong>In your project directory, create a <code>docker-compose.yml</code> file with the following configuration:</strong><br>
<strong>2) Setup for Adguard home</strong>
<pre><code>docker run --name adguardhome\
    --restart unless-stopped\
    -v /my/own/workdir:/opt/adguardhome/work\
    -v /my/own/confdir:/opt/adguardhome/conf\
    -p 53:53/tcp -p 53:53/udp\
    -p 67:67/udp -p 68:68/udp\
    -p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp\
    -p 853:853/tcp\
    -p 784:784/udp -p 853:853/udp -p 8853:8853/udp\
    -p 5443:5443/tcp -p 5443:5443/udp\
    -d adguard/adguardhome
</code></pre>
<strong>Start the Services</strong>
<pre><code>docker start adguardhome</code></pre>
<strong>General you get error on port 53 to solve it run below code.</strong>
<pre><code>sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
docker restart adguardhome</code></pre>

<strong>Now, you can open the browser and navigate to <code> <your_devic_ip>:3000 </code>⁠ to control your AdGuard Home service</strong>

<strong>3. Configure AdGuard Home</strong>
<strong>Once the containers are running, open your browser and go to <code><your_devic_ip>:3000</code> to access the AdGuard Home setup wizard.</strong>
<ul>
    <li><strong>Follow the wizard to complete the configuration of AdGuard Home, setting it up as your DNS server.</strong></li>
   <li><strong>Copy the IPv4 DNS address and paste it into the DNS settings field of the WireGuard client for proper DNS resolution.</strong></li>
</ul>
</ul>

<strong>4. Configure WireGuard Clients</strong>
<ul>
   <li><strong>In your WireGuard client, enter the DNS address that you have copied from the 'Setup Guide' tab of AdGuard Home.</strong></li>
</ul>

<h3>Conclusion</h3>
<strong>With AdGuard Home and WireGuard running in Docker, you now have a secure, private, ad-free browsing experience for your entire network. This setup not only blocks unwanted ads and trackers but also allows you to securely connect to your home network from anywhere using WireGuard.</strong>

<strong>If you have any questions or encounter issues, feel free to open an issue in this repository!</strong>


