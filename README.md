# Hacking Network Protocols: Wi-Fi

Wi-Fi or in tech. term known as 802.11 is the whispering messenger that brings the world to our fingertips. It's a silent, yet powerful force that breaks down the walls of physical limitations, allowing us to browse websites, stream our favorite shows, and chat with friends across the globe - all without a single wire in sight. Thing with Wi-Fi is quite obvious - is everywhere - homes, coffee-shops, campuses, restaurants, gyms you name it - everyone use it so it creates a large vector, basically and open field for hackers to mess with it - which we will do in this project.

# Part 1 - Wi-Fi Overview

Wi-Fi, a term that has become synonymous with wireless internet access, refers to a set of wireless networking technologies that use radio waves to provide high-speed internet and network connections. A small overview of WiFi:

1. **Based on IEEE 802.11 Standards**: Wi-Fi is grounded in 
the IEEE 802.11 standards, a family of protocols developed by the 
Institute of Electrical and Electronics Engineers. These protocols 
define the communication over wireless LAN (Local Area Network).
2. **Operation in Frequency Bands**: Wi-Fi typically operates in the 2.4 GHz and 5 GHz frequency bands. The 2.4 GHz band is more common and has a longer range but is also more prone to interference, while the 5 GHz band provides faster data rates and less interference but has a shorter range.
3. **Data Transmission Using Radio Waves**: Devices like smartphones, laptops, and routers use radio waves to transmit and receive data wirelessly. This allows for the flexibility of moving around within the coverage area while maintaining a network connection.
4. **Network Components**: Essential components of a Wi-Fi network include a wireless router or access point and wireless-enabled devices. The router connects to a broadband connection and transmits a wireless signal that devices can connect to.
5. **Security Protocols**: Security is a crucial aspect of Wi-Fi networks. Protocols like WEP (Wired Equivalent Privacy), WPA (Wi-Fi Protected Access), WPA2, and WPA3 are used to encrypt data and protect the network from unauthorized access.
6. **Applications**: Wi-Fi is used in a variety of settings, including homes, offices, public hotspots, and even in outdoor settings. It's ideal for situations where cabling is impractical or for devices that need to move around within a building or area.
7. **Variants and Speeds**: Different versions of Wi-Fi (like 802.11n, 802.11ac, and 802.11ax) offer varying speeds and capabilities. Newer standards like Wi-Fi 6 (802.11ax) provide improvements in efficiency, speed, and handling multiple devices.

The term ‘Router’ is often times used to describe any home appliance which usually looks like this:

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled.png)

This machine combines multiple roles:

- **Modem** - back in the day modem was separate box which connected to your home router - modem simply convert’s analog signal to digital - this function is now embedded to the modern router’s
- **Router** - to send and receive traffic on the Internet
- **Switch** - to establish wired (physical) connection to the Internet - mostly used for higher speed, low latency, security and other services such IPTV
- **Access Point (AP)** - to establish Wireless connection for Internet access - usually when we connecting to Wi-Fi network we are connecting to AP.

To put it simple - home router isn’t just a router - is a combination of devices which f.e in enterprise environment are separate devices packed with advanced functions -  but just for home use this is just enough. Lets clear some tech. term’s which we need to cover in order to move to the sexy stuff:

- **PSK** - pre-shared key - this is a AP password we use to connect
- **SSID** - name of the AP we see when we searching to the Wi-Fi networks
- **ESSID** - used when multiple AP’s creates one large Wifi network with one name (SSID)
- **BSSID** - unique identifier for every AP - same as AP’s MAC adress
- **Channels** - single pipe which can transmit data over air - Wifi usually operates on channels 1-14
- **WPA** - WiFi protected access - security feature which encrypts ongoing traffic - most used is WPA2 Personal, slowly replaced by WPA-3 which is requirement for Wifi 6

# Part 2 - Hacking Wi-Fi

**!!! DISCLAIMER - this project is dedicated to some of the common Wi-Fi attacks. Performing these attacks are only for educational purposes. I am performing these attacks in the isolated lab which i own and manage. DO NOT PERFORM this type of attacks on any network which you do not own otherwise this will be considered illegal, un-ethical and may result in pressing charges and coliding with authorities in particullar organization or state.**

## Gear up

So we have little understanding how does Wi-Fi works - now lets break it. In this section you will understand how important it not to rely on cheap hardware and default settings which usually comes from your ISP - serving as home router and how important is to at least setup a strong password.

Through this section ill be using ALFA Wi-Fi network card - **AWUS036ACHM.**

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%201.png)

Alfa has good reputation with these card’s used for Wi-Fi penetration testing, most of them work’s out of the box due Kali has pre-installed drivers already, they are in-expensive and reliable. Here is the list of Kali Linux compatible cards:

[https://www.alfa.com.tw/collections/kali-linux-compatible](https://www.alfa.com.tw/collections/kali-linux-compatible)

Another feature which Alfa cards support’s is **monitor mode**.  Most of the Wi-Fi cards operates in three modes:

- **Managed mode (Client mode)** - Wi-Fi card connects to an AP, standard mode for most client devices.
- **Master mode (AP mode)** - Wi-Fi card operates as AP, other devices can connect to it, this mode is used to create hotspots.
- **Monitor mode** - in this mode Wi-Fi card can listen and capture packets on channels without any association with AP, used for network diagnostics, traffic analysis - essential for Wi-Fi pen-testing.

Also we need AP for hacking - ill be using **TP-LINK TL-WR840N** hence is cheap and lot of IPS’s is using it as home appliance. 

[https://www.tp-link.com/in/home-networking/wifi-router/tl-wr840n/#specifications](https://www.tp-link.com/in/home-networking/wifi-router/tl-wr840n/#specifications)

Software which we will be using is **aircrack-ng**. Is a complete set of tools for Wi-Fi security - monitoring, attacking, testing and cracking. By default is part of the Kali so there is no additional installation needed. For more info check link below:

[https://www.aircrack-ng.org/](https://www.aircrack-ng.org/)

Lets fire up Kali and prepare our Wi-Fi card - as we can see Kali recognized the card out of the box and also we can see it when listing wireless network devices.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%202.png)

Lets switch Wi-Fi card into monitor mode and we are good to go

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%203.png)

## De-authentication attack

De-authentication attack is type of DoS (Denial of Services) attack which can effectively subvert the 802.11 design - deauth frame is the management frame which is part of 802.11 protocol - is used to terminate Wi-Fi connection - it can be either sent from AP or from station (client). But it has two major flaw’s - de-authentication frame is ************************notification************************ it means it cannot be rejected - de-authentication is often sent by AP because or reboot or by client because is switching to another AP. Another is that is un-encrypted which means it can be spoofed - we basically need sender or receiver MAC address - which we can easily obtain by running quick scan.

Since Wi-Fi hardly relies on broadcast’s we can easily capture frames from air which are broadcasted from AP’s - lets fire up aircrack.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%204.png)

Our AP has SSID **TP-Link_6F20** and we easily captured the BSSID. We also meanwhile connected the client to the AP and started pinging the default gateway which is our AP in this case. Now we  will send the de-authentication frames onto BSSID and see what happens.

Kali:

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%205.png)

AP client:

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%206.png)

As we can see client has been kicked out of the AP and then logged back after short while. With this attack we can easily create DoS attack to deny any station (client) connect to the network and disrupt clients.

## Attacking WPA2-PSK

WPA2-PSK is most widely used security standard among Wi-Fi routers and AP’s - its encrypting the communication and ensures the data integrity and security. But - there is another flaw - the management packet’s as we already mentioned - are not encrypted. Since WPA-2 is using hashing to hide the password which is flowing over the air we can capture it and crack it.

This process involves sniffing for target BSSID first, start capture then perform client de-authentication and once client will reconnect back we will capture the 4 way handshake which contains password hash which we can crack using aircrack.

We will start sniffing packets and target our BSSID and at the same time send some de-authentication packets to AP.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%207.png)

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%208.png)

We only need 2 or 3 from which client can very easily recover

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%209.png)

Once client reconnected we will capture the handshake in the process, lets examine it in Wireshark.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2010.png)

In Wireshark search for eapol - Extensible Authentication Protocol over LAN - widely used in LAN/WLAN on Layer 2 especially during 4-way handshake using WPA2. In the capture we can see Key Data which is our target.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2011.png)

Lets use aircrack to crack the hashed password from captured handshake. In this case i am using wordlist -  they are special files containing most used or default username and password for various devices which could be scrapped over the Internet. rockyou.txt wordlist is one of the most used English wordlist out there - we will use with aircrack to crack the password.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2012.png)

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2013.png)

Success - it took a fraction of a second to crack the password - hence is really simple and obvious. Lets connect from our Kali machine to target Wi-Fi.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2014.png)

Success we are in, lets now check what IP adress does the default gateway has and run quick nmap scan.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2015.png)

Seems the AP has http port open - lets open a web-browser. As we can see out of the box there is no demand for username just for password - another lack of better security. Lets try same password which we used to connect to Wi-Fi.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2016.png)

Piece of cake - we are inside. Lets see who is connected to AP except us.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2017.png)

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2018.png)

There is another client there, lets use MAC address filtering to ban him from the network.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2019.png)

And done -  client is out and AP is ours. In cases like this where AP is lacking from basic security and traffic flows un-encrypted over the air at least strong password is better step to security.

## Evil Twin

Okay, we successfully hacked the Wi-Fi password and took over - but owner can notice this fairly easily and quickly (who can actually live without Wi-Fi these days, lol) so he will try to restart the AP or call ISP support for help - they will advice him to flip the AP to factory defaults with password on the back of the router and setup stronger password with special characters, numbers even advice him to use un-common words to make cracking more difficult, even forcing him to upgrade to better router which will use username as well.

Cracking involves iterating over wordlist as we did trying to find a match or utilize brute-force tools which will try countless possibilities to find the match - this can take days, even months and we can still fail trying to obtain the password.  They are better ways - **social engineering**.

Renowned cyber-security guru Bruce Schneier in his latest book Hacker’s Mind proclaimed:

“Amateurs hack computers, real professionals hack people”. 

There is endless ways to harden the system to make it almost in-penetrable - but machines are mostly used by humans and humans - are humans - not perfect, making mistakes, having bad day - aspect of social engineering is crucial and often times overlooked. Lets leverage it:

Our attack is called Evil Twin - we will pretend to act as legitimate AP where victim will login and put the Wi-Fi password to perform action on a AP. For this attack we utilize tool called **wifiphisher**. This tool will use the original name of the AP, will put itself as legitimate AP and host fake http website - similar to AP’s management GUI - to subvert the important message to the victim and grab the password in process. See official documentation for more: [https://wifiphisher.org/](https://wifiphisher.org/)

End of theory, lets hack - i found that the wifiphisher is not part of the Kali by default so lets install it first.

**Important: I was not lucky make it work with latest version of Kali Linux offered on OffSec site - version which worked for me is 2022-3.**

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2020.png)

Ok is there - next we need to re-scan the AP for some details.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2021.png)

With these details run the wifiphisher - from these options Firmware update seems as most trusted option - term Firmware is known among most non-technical people so we have a good chance for success. Important aspect of social engineering is to build an trust and appear important.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2022.png)

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2023.png)

Ok attack is running and we can already see victim reconnected based on http messages. For now victim only GETs the website. Soon the webpage with Firmware Update will pop.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2024.png)

Looks pretty trustworthy to me - lets put the password in and click Start Upgrade.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2025.png)

As soon as we start the upgrade - POST request is showed with the password. Pretty scary easy.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2026.png)

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2027.png)

Connected - again.

![Untitled](Hacking%20Network%20Protocols%20Wi-Fi%20d38529adb9814e0889a0b8eeddc6c90f/Untitled%2028.png)

So this is how we can pretty easily obtain even the very strong password by attacking the very nature of human.

# Wrap up

In this lab i demonstrated how easy we can use various ways to obtain Wi-Fi password or disrupt the network completly - nothing can be perfectly secure - but we can create a multiple roadblocks which hacker needs to pass in order to get to us - make it always hard and frustrating and time consuming for the evil side - few tips how to mitigate:

- **use strong password** - more than set of random characters use 5-7 random un-conventional words (u can use different language for example Italian) to increase entropy and less appear in popular wordlists
- **use better gear** - dont rely on cheap appliance which does not support proper authentification using username - username is another roadblock for attacker.
- **use 5GHz** - i do not say there is no attack against 5GHz - but most of the people do not even know about another band outside of basic 2.4GHz - and most of the attacks are against 2.4GHz - bonus you will get faster bandwidth.
- **use WPA3** - also WPA3 has vectors which can make it vulnerable, is using better encryption and does not use PSK so is harder to crack
- **use WPA2-Enterprise** - enhanced version of WPA2-Personal which is used for most home networks - this is for more techies, WPA2-Ent uses separate authentication for each user, utilizing Radius - authentication protocol which is running as separate service on another machine.

Aaand that would be it for the Wi-Fi hacking, i hope you found it useful and educating but most important fun. Cheers

## Resources:

[https://en.wikipedia.org/wiki/IEEE_802.11](https://en.wikipedia.org/wiki/IEEE_802.11)

[https://en.wikipedia.org/wiki/IEEE_802.11#Layer_2_–_Datagrams](https://en.wikipedia.org/wiki/IEEE_802.11#Layer_2_%E2%80%93_Datagrams)

[https://blog.spacehuhn.com/wifi-deauthentication-frame](https://blog.spacehuhn.com/wifi-deauthentication-frame)

[https://www.alfa.com.tw/collections/kali-linux-compatible](https://www.alfa.com.tw/collections/kali-linux-compatible)

[https://www.tp-link.com/in/home-networking/wifi-router/tl-wr840n/#specifications](https://www.tp-link.com/in/home-networking/wifi-router/tl-wr840n/#specifications)

[https://www.aircrack-ng.org/](https://www.aircrack-ng.org/)

[https://wifiphisher.org/](https://wifiphisher.org/)

[https://old.kali.org/kali-images/kali-2022.3/](https://old.kali.org/kali-images/kali-2022.3/)

[https://www.goodreads.com/book/show/123849592-network-basics-for-hackers](https://www.goodreads.com/book/show/123849592-network-basics-for-hackers)

[https://www.goodreads.com/book/show/1153850.Network_Warrior](https://www.goodreads.com/book/show/1153850.Network_Warrior)

[https://www.youtube.com/watch?v=TZcgvi_KRvY](https://www.youtube.com/watch?v=TZcgvi_KRvY)