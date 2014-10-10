# BitHammer

<img align="right" alt="BitHammer, The BanHammer for BitTorrent" src="http://i.imgur.com/s84Kca4.jpg" />

    NOTE: BitHammer is FOR RESEARCH 
          PURPOSES ONLY.
      Get permission from a network's 
          owner before using it.  
      You assume all responsibility 
          for its use.
          

#### The BitTorrent BanHammer.

BitTorrent is ok.  Using BitTorrent on your own network is your own business.  Using BitTorrent on public wifi is not.  BitTorrent aggressively soaks up bandwidth and connections, rendering other programs useless, and other users and admins frustrated.

http://askubuntu.com/questions/16680/using-a-bittorrent-client-slows-down-internet-connection

Over a year of traveling, I've consistently struggled with broken wifi spots overloaded with torrent traffic.  After talking with the frustrated non-technical people who owned/managed them, I wrote this program to help network users and owners.

This program:
  1. Listens for BitTorrent clients on the network, 
  2. Adds their IPs and MACs to a ban list, 
  3. Bans them from the network for as long as the program is running.

## BitHammer on Linux:l

Install `python`, `scapy`, and `git` :

    sudo apt-get install python python-scapy git

Run the program:

    git clone git@github.com:MichaelJCole/bithammer.git
    cd bithammer
    sudo python ./bithammer

The `scapy` network library requires `sudo` for root priviledges.  Built and tested with Python 2.7.6.  This is my first Python program :)

## BitHammer on Windows or MacOS

In theory, BitHammer should work outside Linux, but I don't care to test it.  Code and documentation pull requests are welcome.  [This may help]( http://www.secdev.org/projects/scapy/doc/installation.html#platform-specific-instructions) to install `scapy` - the networking library used.

## How does it work?

The program listens for BitTorrent users advertising themselves via LPD:  

    http://en.wikipedia.org/wiki/Local_Peer_Discovery

The program then bans those users from the network via ARP Cache poisoning:

    http://en.wikipedia.org/wiki/ARP_spoofing

Network Traffic to/from the banned computer is redirected to your computer, which your computer ignores because it's improperly addressed.  This effectively bans the target from the network.

ARP cache poisoning continues as long as the program is running.  Restarting the program clears the ban list.  It may take some time for the ban to clear.

## Is this legal?

I am not a lawyer.  I recommend the courtesy of getting permission before using it on someone else's network.

## BitHammer isn't working

There are a variety of reasons it may not work as you would like.  

 - The wifi access point may isolate users from each other, preventing LPD
 - BitTorrent clients may be on another sub-net, preventing LPD
 - BitTorrent clients may not be advertising with LDP
 - Your bandwidth problem may unrelated to BitTorrent
 - Something else

## I'm banned!  Are you anti-bittorrent, pro-hitler, and anti-puppies?

Climate change, ocean fishery collapse, and crappy wifi are all tragedies of the commons.

http://en.wikipedia.org/wiki/Tragedy_of_the_commons

It's not personal.  It's just that your ignorance and selfishness are ruining things for everyone else.

__The best workaround to BitHammer is to get a 'private internet VPN'__ and run your BitTorrent traffic through that.  Google it.  This will:

  1. Prevent BitHammer from banning you, 
  2. Keep your usage to levels home internet routers can handle, 
  3. Prevent the WiFi owner from getting file-sharing letters from lawyers.

## Thank you! and credit where credit is due:

https://github.com/evilsocket/NetCommander - Python code for ARP cache poisoning and MITM attack. 
https://github.com/jonls/lpd-monitor - Library for LPD monitoring.  

## Are there better alternatives to BitHammer?

Yes, alternatives include:

  1. Block BitTorrent on your router
  2. Configuring traffic QOS on the router
  2. Banning users on the router

These are advanced router configurations and may be confusing to the average user and 'home' router equipment:

Login to your router with the admin password:

  1. Get your router's IP address (e.g. 192.168.1.1)
  2. Open http://192.168.1.1 in a web browser
  3. Login with your admin password

Configure your router:
  
  1. To block BitTorrent, block ports 6881 to 6999 (easy, but not always effective)
  2. To block devices, use the MAC ids from BitHammer: http://blog.dlink.com/how-to-block-devices-from-your-home-network/ 
  3. To slow down BitTorrent so other traffic takes precedence, configure "Traffic Shaping" or "Quality of Service (QoS)"; this depends on your router.
