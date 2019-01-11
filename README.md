<head>
  <link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
  <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
  <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
  <link rel="manifest" href="/site.webmanifest">
  <link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5">
  <meta name="msapplication-TileColor" content="#da532c">
  <meta name="theme-color" content="#ffffff">
</head>

## Welcome to Shrubhog's website
I am going to try my best to update this as many times as possible.
It's mainly going to be a concept notesheet as a backbone for my repos, so it won't have much.

### September 19, 2018
I've just started a little template engine for text-based monster fighting.

### September 29, 2018
Time to start working on the thing again.
I've added things. Lots of them.
## September 30, 2018
I'm thinking of creating an incremental game, which is my favorite game style, based on coffee. First, you're a man picking money from his couch, then you decide to buy a coffee machine. Make coffee for your neighbors, expand, upgrade, get a permanent  place, and maybe even split off into a coffee country. But it's just an idea. For now.
## October 8, 2018
Already made the coffee thing a few days ago.
## November 16, 2018
I've been busy. I've made my first release of the game, [Expresso](https://shrubhog.github.io/expresso), and committed over 60 times to it. Now, it has favicons, but the basic style of the game is still really non-existent.
## November 30, 2018
Expresso features are in a dry spell, so I've been thinking about making another game about leaves. I know it's too late for a game about fall, but a game is a game. Game Idea: 
* You are a kid collecting leaves for a big leaf pile
* You can hire other kids to collect leaves by sweeping up fallen ones, throwing sticks at branches, and shaking trees.
* Eventually, your thirst for leaves becomes so great you harvest whole planets for their leaves.
* You can upgrade your employees' equipment to increase leaf production. (e.g. short stick &rarr; long stick &rarr; ... &rarr; steel plant-sensing boomerang)
* In the end, to ascend (restart the game with perks), you jump in the leaf pile.

I've been thinking about it a lot, and I'm even thinking of adding a tabs with Bootstrap. I have to do it in the beginning, though, because I don't want to have to rip the whole thing apart and put it back together after I've finished the game.

## January 4 2019
Added tabs to [Expresso](https://github.com/Shrubhog/expresso). New Years Resolution: commit more.
## January 11, 2019
Added [Jemoji](https://github.com/jekyll/jemoji) to the Pages. :smile:
I was sitting down today and learning about the Live Coding features on VS Code, and came across the Server Sharing section. Because of my train of thought being more like a flying bouncy castle, I thought about the indie game, Minecraft, and its LAN server capabilities. So I booted up Minecraft and set to work to see what I could do with it.
### Test One
The first test was starting a LAN server on one of my worlds and copied the server port. A quick visit to [localhost:44169](localhost:44169) with Google Chrome returned an invalid response, which boggled me since I had never really thought about other protocols for any kind of servers (FTP is one I had heard of).
### Test Two
Next, I opened the terminal in Ubuntu 18.04 and tried to ping my own server.
```console
foo@bar:~$ ping localhost:44169
ping: localhost:44169: Name or service not known
```
And, I am a dummy. I switched to nping and got a real response.
```console
foo@bar:~$ nping -p 44169 127.0.0.1
Starting Nping 0.7.60 ( https://nmap.org/nping ) at 2019-01-11 14:37 CST
SENT (0.0021s) Starting TCP Handshake > 127.0.0.1:44169
RCVD (0.0021s) Handshake with 127.0.0.1:44169 completed
SENT (1.0035s) Starting TCP Handshake > 127.0.0.1:44169
RCVD (1.0036s) Handshake with 127.0.0.1:44169 completed
SENT (2.0048s) Starting TCP Handshake > 127.0.0.1:44169
RCVD (2.0049s) Handshake with 127.0.0.1:44169 completed
SENT (3.0061s) Starting TCP Handshake > 127.0.0.1:44169
RCVD (3.0062s) Handshake with 127.0.0.1:44169 completed
SENT (4.0075s) Starting TCP Handshake > 127.0.0.1:44169
RCVD (4.0076s) Handshake with 127.0.0.1:44169 completed
 
Max rtt: 0.078ms | Min rtt: 0.018ms | Avg rtt: 0.054ms
TCP connection attempts: 5 | Successful connections: 5 | Failed: 0 (0.00%)
Nping done: 1 IP address pinged in 4.01 seconds
```
I then had a glimpse into what protocol Minecraft used: TCP. Luckily, there is an extremely helpful wiki called [Minecraft Modern](wiki.vg) for developers which gave me further documentation in the Minecraft protocol. Unfortunately, it's pretty dense and is written for a Java parser, which is understandable considering the game's base language. I decided to toss the wiki and settle for my TCP answer.
### Python Handshaking
I wrote a TCP sending and recieving script (I say wrote but I took it from [here](https://wiki.python.org/moin/TcpCommunication)).
```python
#!/usr/bin/env python

import socket

TCP_IP = "127.0.0.1"
TCP_PORT = 44169
BUFFER_SIZE = 1024
MESSAGE = "Hi, Minecraft"

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((TCP_IP, TCP_PORT))
s.send(MESSAGE)
data = s.recv(BUFFER_SIZE)
s.close()

print "It said:", data
```
Upon running this, it ran for a while (to timeout) and returned a blank response. Huh.
