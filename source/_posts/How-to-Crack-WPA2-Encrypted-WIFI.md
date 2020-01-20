---
title: How to Crack WPA2 Encrypted  WIFI
date: 2019-11-16 14:48:54
tags: WPA2 Encryption
categories: Info
description: "Many WPA2 encrypted networks use PSK (Pre-Shared Key) as the method for authentication to the
              network and AES as the encryption method for the network which is provided by the router. Due to the ubiquity of WPA2-PSK 
              encryption for home routers, that's what will be discussed in this article: How to crack WPA2 encrypted WIFI."
---

<p><i>Disclaimer: This information is purely for educational purposes. <strong> DO NOT </strong> try any of the techniques 
described in this article unless you have proper authorization by the network admin to do so <strong>OR </strong> 
you own the network. Accessing a network without authorization is a <strong>CRIME</strong>.</i></p>

<h2><u>What is WPA2 Encryption?</u></h2>

<p class = "tab" >WPA2 encryption is  short for "Wi-Fi Protected Access 2" which is a method used to secure a network. 
WPA2 encryption is currently the most popular wireless encryption where more than 60% of people use it according to 
<a href="https://wigle.net/stats#" >Wigle.net</a> (as of the date of this article, the link should provide a more 
accurate percentage).
Other protocols that make up a smaller percentage are WEP, WPA, and WPA3 (currently a next-gen protocol). </p>

<p class = "tab">Many WPA2 encrypted networks use PSK (Pre-Shared Key) as the method for authentication to the
network and AES as the encryption method for the network which is provided by the router. Due to the ubiquity of WPA2-PSK 
encryption for home routers, that's what will be discussed in this article: How to crack WPA2 encrypted WIFI.</p>

Things you'll need:
<ul>

<li>Computer with Kali Linux (Virtual Machine, Dual boot, or whichever way you choose)</li>
<li>Wireless adapter capable of using Monitor mode and compatible with Kali Linux</li>
<li>Router that YOU OWN or have the owner's permission to use (See Disclaimer)</li>
<li>Another device to connect to the router or already connected to the router (phone, another virtual machine, another computer, etc.)</li>

</ul>

<h2><u>Overview of Cracking WIFI Password</u></h2>
<p class="tab"> Before we dive into how to crack WIFI passwords, let's first go over briefly how this process works.
Basically, when a device connects to the router with WPA2, there's a handshake that occurs during the connection. This 
handshake, if captured can be used to figure out the password for the WPA2 encryption. So, in the following methods, our 
goal will be to capture this handshake when a device connects to the router, so we can then take this handshake and 
use it to find the password to connect to the WIFI. One of the limitations of this brute force method is you are 
guessing different passwords until you get the right one, so unless you guess the correct password, you won't be able to 
get the password. Thus, having a good password list is essential or having some clues to what the password might be 
so you can make your own password list to use. 
Alright, let's get started!
 </p>
 
 <h2><u>1. Setting Up Monitor Mode</u></h2>
 <p class="tab">The first step is to set your wireless adapter to monitor mode so we can see the packets. Please note 
 that I would not recommend you use the internal wifi card of your computer to do this for a couple of reasons: 1. 
 most internal wifi cards are not compatible with Kali Linux and/or do not support monitor mode. 2. If you use the
 internal wifi card of your computer in monitor mode, you won't be able to connect to wifi from that wifi card. Thus, 
 I would instead recommend using an external wifi card that is compatible with Kali Linux and capable of monitor mode. 
 </p>
 
 <p> First, we'll need to see our network interfaces to make sure Kali can see the wireless adapter we're 
 using. By entering the command below, we'll be able to see the network interfaces connected to the system.
 <br>
 <br>
 <code>ifconfig</code>
 <br>
 <br>
 <img src="/img/crackWPA2/ifconfig pic.png" height="50%" width="50%">
 
 <br>
 As we can see in the image, <b>wlan0</b> is the wireless adapter I'm using for this demonstration. The eth0 is the bridge
 for the internet connection in the virtual machine.
 </p>
 
 <p>
 Next, we'll set our external wifi card to monitor mode. To do this, we'll do the following process.
 </p>
 
 <p>First, check the wireless network interfaces with the following command: </p>
 <br>
 <code>iwconfig</code>
 <br>
 <br>
 <img src="/img/crackWPA2/iwconfig.png" height="50%" width="50%">
 <br>
 
 <p>To set our external wifi card to monitor mode, use the following commands:</p>
 <code>ifconfig wlan0 down</code> <br>
 <code>iwconfig wlan0 mode monitor</code><br>
 <code>ifconfig wlan0 up</code><br>
 <br>
 <p>Then, we'll check to make sure the adapter is in monitor mode:</p>
 <code>iwconfig</code><br>
 <br>
 
 <p>After this, you should have the same as in the following picture:</p>
 <img src="/img/crackWPA2/monitor_mode.png" height="50%" width="50%">
 
 <h2><u>2. Getting the Handshake</u></h2>
 <p>The first thing we'll need to do is make sure to kill ALL of the potentially problematic pids with the following
 command:</p>
 <code>airmon-ng check kill</code>
 <br>
 <br>
 <p>After killing potentially problematic pids, we can now start to capture packets with the following command:</p>
 <code>airodump-ng wlan0</code>
 <br>
 <br>
<p>After running this command, you should see the different packets that your wireless adapter is able to pick up. 
<i><br>Note: airodump-ng by default only scans 2.4GHz channels which is what will be used for this tutorial.</i>
</p>
<br>
<p>You should end up with a table similar to the following, showing the nearby network packets captured:</p>
<img src="/img/crackWPA2/wifi_networks.png" height="50%" width="50%">
<br>
<br>
<p>Next, we'll focus on our target wifi network and to capture packets on for this network. The following command, does 
this after we provide the necessary information.</p>
<p>General Format:</p>
<code>airodump-ng -c (channel number) --bssid (bssid of target network) -w (path of file for captured data) (wireless adapter name)</code>
<br>
<br>
<p>Example:</p>
<code>airodump-ng -c 1 --bssid B2:72:BF:DA:1C:9D -w /root/Documents/handshake wlan0</code>
<br>
<br>
<img src="/img/crackWPA2/airodump_no_handshake.png" height="70%" width="70%">
<p class="tab">After this step, your computer will be monitoring the packets on the network you specified waiting for a handshake to 
occur when a device connects to the network. So until a device connects to the network and your wireless adapter is able 
to capture the handshake, you'll have to wait. Another option is to deauthenticate a device already connected to the 
network so when it reconnects to the network, you can capture the handshake (see the deauthencation section below for
how to do this. When the handshake is captured, in the upper right-hand corner, you should see "WPA Handshake: " 
followed by the BSSID of the network you're monitoring.</p>
<img src="/img/crackWPA2/airodump_handshake.png" height="70%" width="70%">
<br>
<p>After getting the handshake, it should be saved to the path you used in the previous command. Next, we're 
   going to crack the password using the handshake we got and saved.</p>

<h4>Deauthentication</h4>
<p class="tab">When deauthenticating an accessing point, the deauth packets can be sent to either a specific device on the network if you know 
the MAC of the device or deauth ever device on the network if you don't know the specific MAC address of a device. We'll 
be deauthenticating every device on the network for this since it's a simpler command and doesn't require us to find the 
MAC address of a connected device. One important thing to note about this is there must be a device connected to the 
access point (router) in order for us to deauth a device which forces it to reauthenticate, allowing us to capture the 
handshake. To deauthenicate devices from a router, run the following command in a new terminal ( leave the terminal you
have open from the previous step that's listening to the traffic of the target network):</p>
<p>Example:</p>
<code> aireplay-ng -0 100 -a B2:72:BF:DA:1C:9D wlan0</code>
<ul>
<li><strong>-0</strong> means deauthenticate</li>
<li><strong>100</strong> is number of deauth packets to send; if 0 means it'll continuously send deauth packets </li>
<li><strong>-a B2:72:BF:DA:1C:9D</strong> is the MAC address of the access point (router) </li>
<li><strong>wlan0</strong> is our wireless interface name</li>
</ul>
<p>After running the deauth command successfully, you should get something similar to this:</p>
<img src="/img/crackWPA2/deauth.png">

<h2><u>3. Cracking the Password</u></h2>
<p class="tab">There are many options of different software for cracking passwords such as Ophcrack, John the Ripper, Hashcat, etc. 
However, we'll be using aircrack-ng for this tutorial. One of the most important things when brute forcing a password is 
to have a good word list. Since if the correct password isn't in the list, you won't be getting the password. So your 
choices here are to either use a pre-made list of passwords or if you have an idea of what the password could be, you can
also create your own password list using something like Crunch. For this tutorial, I'll be using the rockyou.txt list 
that comes pre-downloaded on Kali Linux (you will have to unzip the file first to get the text file in the path: 
/usr/share/wordlists).</p>

<p>To use aircrack-ng to brute force the password, use the following:</p>
<p>General Format:</p>
<code>aircrack-ng -w (word list path) -b (bssid of target network) (path of handshake file)</code>
<br>
<br>
<p>Example:</p>
<code>aircrack-ng -w /usr/share/wordlists/rockyou.txt -b B2:72:BF:DA:1C:9D handshake-01.cap</code>
<br>
<br>
<p>After running the above command, each password in the list you provided will be tested until there is a match. 
Afterwards, you should see something as below:</p>
<img src="/img/crackWPA2/crack_pic.png" height="70%" width="70%">
<br>
<br>
<p>As we can see in the above picture, the password for my wifi network I was testing is "password" and it only took 
1 second to find the password using the rockyou word list. This is just to show for those out there with a easy to guess or common password, 
you should change your password to something that isn't simple or easy to guess :)</p>
