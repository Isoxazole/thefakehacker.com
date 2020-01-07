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

<li>Computer with Kali Linux (Virtual Machine or Dual boot)</li>
<li>Wireless adapter capable of using Monitor mode</li>
<li>Router that YOU OWN or have the owner's permission to use (See Disclaimer)</li>
<li>Another device to connect to the router (phone, another virtual machine, another computer, etc.</li>

</ul>

<h2><u>Overview of Cracking WIFI Password</u></h2>
<p class="tab"> Before we dive into how to crack WIFI passwords, let's first go over briefly how this process works.
Basically, when a device connects to the router with WPA2, there's a handshake that occurs during the connection. This 
handshake, if captured can be used to figure out the password for the WPA2 encryption. So, in the following methods, our 
goal will be to capture this handshake when a device connects to the router, so we can then take this handshake and 
use it to find the password to connect to the WIFI. Alright, let's get started!
 </p>
 
 