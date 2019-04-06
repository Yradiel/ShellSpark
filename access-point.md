
<article class="entry-content">
    <h1>Setting up a Raspberry Pi as an access point in a standalone network (NAT)</h1>
<p>The Raspberry Pi can be used as a wireless access point, running a standalone network. This can be done using the inbuilt wireless features of the Raspberry Pi 3 or Raspberry Pi Zero W, or by using a suitable USB wireless dongle that supports access points. </p>
<p>Note that this documentation was tested on a Raspberry Pi 3, and it is possible that some USB dongles may need slight changes to their settings. If you are having trouble with a USB wireless dongle, please check the forums.</p>
<p>To add a Raspberry Pi-based access point to an existing network, see <a href="#internet-sharing">this section</a>.</p>
<p>In order to work as an access point, the Raspberry Pi will need to have access point software installed, along with DHCP server software to provide connecting devices with a network address. Ensure that your Raspberry Pi is using an up-to-date version of Raspbian (dated 2017 or later).</p>
<p>Use the following to update your Raspbian installation:</p>
<pre><code>sudo apt-get update
sudo apt-get upgrade</code></pre>
<p>Install all the required software in one go with this command: </p>
<pre><code>sudo apt-get install dnsmasq hostapd</code></pre>
<p>Since the configuration files are not ready yet, turn the new software off as follows: </p>
<pre><code>sudo systemctl stop dnsmasq
sudo systemctl stop hostapd</code></pre>
<p>To ensure that an updated kernel is configured correctly after install, reboot:</p>
<pre><code>sudo reboot</code></pre>
<h2>Configuring a static IP</h2>
<p>We are configuring a standalone network to act as a server, so the Raspberry Pi needs to have a static IP address assigned to the wireless port. This documentation assumes that we are using the standard 192.168.x.x IP addresses for our wireless network, so we will assign the server the IP address 192.168.4.1. It is also assumed that the wireless device being used is <code>wlan0</code>.</p>
<p>To configure the static IP address, edit the dhcpcd configuration file with: </p>
<pre><code>sudo nano /etc/dhcpcd.conf</code></pre>
<p>Go to the end of the file and edit it so that it looks like the following:</p>
<pre><code>interface wlan0
    static ip_address=192.168.4.1/24
    nohook wpa_supplicant
</code></pre>
<p>Now restart the dhcpcd daemon and set up the new <code>wlan0</code> configuration:</p>
<pre><code>sudo service dhcpcd restart
</code></pre>
<h2>Configuring the DHCP server (dnsmasq)</h2>
<p>The DHCP service is provided by dnsmasq. By default, the configuration file contains a lot of information that is not needed, and it is easier to start from scratch. Rename this configuration file, and edit a new one:</p>
<pre><code>sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig  
sudo nano /etc/dnsmasq.conf</code></pre>
<p>Type or copy the following information into the dnsmasq configuration file and save it:</p>
<pre><code>interface=wlan0      # Use the require wireless interface - usually wlan0
  dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h</code></pre>
<p>So for <code>wlan0</code>, we are going to provide IP addresses between 192.168.4.2 and 192.168.4.20, with a lease time of 24 hours. If you are providing DHCP services for other network devices (e.g. eth0), you could add more sections with the appropriate interface header, with the range of addresses you intend to provide to that interface.</p>
<p>There are many more options for dnsmasq; see the <a href="http://www.thekelleys.org.uk/dnsmasq/doc.html">dnsmasq documentation</a> for more details.</p>
<h2>Configuring the access point host software (hostapd)</h2>
<p>You need to edit the hostapd configuration file, located at /etc/hostapd/hostapd.conf, to add the various parameters for your wireless network. After initial install, this will be a new/empty file.</p>
<pre><code>sudo nano /etc/hostapd/hostapd.conf</code></pre>
<p>Add the information below to the configuration file. This configuration assumes we are using channel 7, with a network name of NameOfNetwork, and a password AardvarkBadgerHedgehog. Note that the name and password should <strong>not</strong> have quotes around them. The passphrase should be between 8 and 64 characters in length.</p>
<p>To use the 5 GHz band, you can change the operations mode from hw_mode=g to hw_mode=a. Possible values for hw_mode are:</p>
<ul>
<li>a = IEEE 802.11a (5 GHz)</li>
<li>b = IEEE 802.11b (2.4 GHz)</li>
<li>g = IEEE 802.11g (2.4 GHz)</li>
<li>ad = IEEE 802.11ad (60 GHz). </li>
</ul>
<pre><code>interface=wlan0
driver=nl80211
ssid=NameOfNetwork
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=AardvarkBadgerHedgehog
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP</code></pre>
<p>We now need to tell the system where to find this configuration file.</p>
<pre><code>sudo nano /etc/default/hostapd</code></pre>
<p>Find the line with #DAEMON_CONF, and replace it with this:</p>
<pre><code>DAEMON_CONF="/etc/hostapd/hostapd.conf"</code></pre>
<h2>Start it up</h2>
<p>Now start up the remaining services:</p>
<pre><code>sudo systemctl start hostapd
sudo systemctl start dnsmasq</code></pre>
<h3>Add routing and masquerade</h3>
<p>Edit /etc/sysctl.conf and uncomment this line:</p>
<pre><code>net.ipv4.ip_forward=1</code></pre>
<p>Add a masquerade for outbound traffic on eth0:</p>
<pre><code>sudo iptables -t nat -A  POSTROUTING -o eth0 -j MASQUERADE</code></pre>
<p>Save the iptables rule.</p>
<pre><code>sudo sh -c "iptables-save &gt; /etc/iptables.ipv4.nat"</code></pre>
<p>Edit /etc/rc.local and add this just above "exit 0" to install these rules on boot.</p>
<pre><code>iptables-restore &lt; /etc/iptables.ipv4.nat</code></pre>
<p>Reboot</p>
<p>Using a wireless device, search for networks. The network SSID you specified in the hostapd configuration should now be present, and it should be accessible with the specified password.</p>
<p>If SSH is enabled on the Raspberry Pi access point, it should be possible to connect to it from another Linux box (or a system with SSH connectivity present) as follows, assuming the <code>pi</code> account is present:</p>
<pre><code>ssh <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="22524b62131b100c13141a0c160c13">[email&#160;protected]</a></code></pre>
<p>By this point, the Raspberry Pi is acting as an access point, and other devices can associate with it. Associated devices can access the Raspberry Pi access point via its IP address for operations such as <code>rsync</code>, <code>scp</code>, or <code>ssh</code>.</p>
<h2>----------------------------------------------------------------------------------</h2>
<p><a name="internet-sharing"></a></p>
<h2>Using the Raspberry Pi as an access point to share an internet connection (bridge)</h2>
<p>One common use of the Raspberry Pi as an access point is to provide wireless connections to a wired Ethernet connection, so that anyone logged into the access point can access the internet, providing of course that the wired Ethernet on the Pi can connect to the internet via some sort of router.</p>
<p>To do this, a 'bridge' needs to put in place between the wireless device and the Ethernet device on the access point Raspberry Pi. This bridge will pass all traffic between the two interfaces. Install the following packages to enable the access point setup and bridging.</p>
<pre><code>sudo apt-get install hostapd bridge-utils</code></pre>
<p>Since the configuration files are not ready yet, turn the new software off as follows: </p>
<pre><code>sudo systemctl stop hostapd</code></pre>
<p>Bridging creates a higher-level construct over the two ports being bridged. It is the bridge that is the network device, so we need to stop the <code>eth0</code> and <code>wlan0</code> ports being allocated IP addresses by the DHCP client on the Raspberry Pi.</p>
<pre><code>sudo nano /etc/dhcpcd.conf</code></pre>
<p>Add <code>denyinterfaces wlan0</code> and <code>denyinterfaces eth0</code> to the end of the file (but above any other added <code>interface</code> lines) and save the file.</p>
<p>Add a new bridge, which in this case is called <code>br0</code>.</p>
<pre><code>sudo brctl addbr br0</code></pre>
<p>Connect the network ports. In this case, connect <code>eth0</code> to the bridge <code>br0</code>.</p>
<pre><code>sudo brctl addif br0 eth0</code></pre>
<p>Now the interfaces file needs to be edited to adjust the various devices to work with bridging. <code>sudo nano /etc/network/interfaces</code> make the following edits.</p>
<p>Add the bridging information at the end of the file.</p>
<pre><code># Bridge setup
auto br0
iface br0 inet manual
bridge_ports eth0 wlan0
</code></pre>
<p>The access point setup is almost the same as that shown in the previous section. Follow <em>all</em> the instructions in the <code>Configuring the access point host software (hostapd)</code> section above to set up the <code>hostapd.conf</code> file and the system location, but add <code>bridge=br0</code> below the <code>interface=wlan0</code> line, and remove or comment out the driver line. The passphrase must be between 8 and 64 characters long. </p>
<p>To use the 5 GHz band, you can change the operations mode from 'hw_mode=g' to 'hw_mode=a'. The possible values for hw_mode are:</p>
<ul>
<li>a = IEEE 802.11a (5 GHz)</li>
<li>b = IEEE 802.11b (2.4 GHz)</li>
<li>g = IEEE 802.11g (2.4 GHz)</li>
<li>ad = IEEE 802.11ad (60 GHz). Not available on Raspberry Pi.</li>
</ul>
<pre><code>interface=wlan0
bridge=br0
#driver=nl80211
ssid=NameOfNetwork
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=AardvarkBadgerHedgehog
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP</code></pre>
<p>Now reboot the Raspberry Pi.</p>
<pre><code>sudo reboot</code></pre>
<p>There should now be a functioning bridge between the wireless LAN and the Ethernet connection on the Raspberry Pi, and any device associated with the Raspberry Pi access point will act as if it is connected to the access point's wired Ethernet.</p>
<p>The <code>ifconfig</code> command will show the bridge, which will have been allocated an IP address via the wired Ethernet's DHCP server. The <code>wlan0</code> and <code>eth0</code> no longer have IP addresses, as they are now controlled by the bridge. It is possible to use a static IP address for the bridge if required, but generally, if the Raspberry Pi access point is connected to a ADSL router, the DHCP address will be fine.</p></article>
