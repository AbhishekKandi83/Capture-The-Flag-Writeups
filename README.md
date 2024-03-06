# Capture-The-Flag-Writeups

## Task : Router |port|
Question: There's some unusual traffic on the daytime port, but it isn't related to date or time requests. Analyze the packet capture to retrieve the flag.

Flag: `VishwaCTF{K3Y5_CAN_0P3N_10CK5}`

We are given a pcap file that has several packets with different protocols. The description mentioned something about daytime port, so I checked online on what port is it to filter my search. Doing some Googling, the port for daytime protocol is `Port 13 (tcp/udp)`. Looking at the packets, there seems to be encoded text that holds valuable information. This is pretty guessy but I tried Vigenère decode with this [site](https://www.guballa.de/vigenere-solver) and it shows that it was decoded using keyword `nnn`.
The decoded text:
```
Hey, mate!
Yo, long time no see! You sure this mode of communication is still safe?
Yeah, unless someone else is capturing network packets on the same network we're using. Anyhow, our text is encrypted, and it would be difficult to interpret. So let's hope no one else is capturing. What's so confidential that you're about to share? It's about cracking the password of a person with the username 'Anonymous.'
Oh wait! Don't you know I'm not so good at password cracking?
Yeah, I know, but it's not about cracking. It's about the analysis of packets. I've completed most of the job, even figured out a way to get the session key to decrypt and decompress the packets. Holy cow! How in the world did you manage to get this key from his device? Firstly, I hacked the router of our institute and closely monitored the traffic, waiting for 'Anonymous' to download some software that requires admin privilege to install. Once he started the download, I, with complete control of the router, replaced the incoming packets with the ones I created containing malicious scripts, and thus gained a backdoor access to his device. The further job was a piece of cake.
Whoa! It's so surprising to see how much you know about networking or hacking, to be specific.
Yeah, I did a lot of research on that. Now, should we focus on the purpose of this meet? Yes, of course. So, what should I do for you?
Have you started the packet capture as I told you earlier?
Yes, I did. Great! I will be sending his SSL key, so find the password of 'Anonymous.' Yes, I would, but I need some details like where to start. The only details I have are he uses the same password for every website, and he just went on to register for a CTF event.
Okay, I will search for it. Wait a second, I won't be sending the SSL key on this Daytime Protocol port; we need to keep this untraceable. I will be sending it through FTP. Since the file is too large, I will be sending it in two parts. Please remember to merge them before using it. Additionally, some changes may be made to it during transfer due to the method I'm using. Ensure that you handle these issues.
Okay! ...
```
Reading the decoded text, it seems that the flag is the password of the user `Anonymous` and the hacker managed to steal a SSL key to encrypt packets. It also mentions that the SSL key is sent to another hacker via FTP. So I filtered the pcap to find ftp-data and found 2 packets, so the key is probably broken up into 2 parts.

<img width="631" alt="image" src="https://github.com/AbhishekKandi83/Capture-The-Flag-Writeups/assets/140315150/bcda8af3-598e-4575-b980-a3c743685e17">

<img width="567" alt="image" src="https://github.com/AbhishekKandi83/Capture-The-Flag-Writeups/assets/140315150/598a9863-dfdf-4e97-ad9a-df62514f66d6">

 I used [cipher identifer] (https://www.dcode.fr/cipher-identifier) to know the cipher - it was vigenere cipher with the same site and succesfully extracted both PSK log files and placed them into Wireshark. This can be done by navigating to `Preferences > Protocols > TLS > (Pre)-Master-Secret log files` and add in the key file.

After decoding it, several HTTP2 and HTTP3 packets will be found. Filtering down for passwords. we get thr flag.

<img width="413" alt="image" src="https://github.com/AbhishekKandi83/Capture-The-Flag-Writeups/assets/140315150/4f2a3668-0604-4c1a-8b40-54944c6ef560">

<img width="602" alt="image" src="https://github.com/AbhishekKandi83/Capture-The-Flag-Writeups/assets/140315150/f0ec8e49-83f8-46ac-a9b4-c835f5c0ff5a"> we see the flag 

