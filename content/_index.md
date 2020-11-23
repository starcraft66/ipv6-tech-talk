+++
title = "Restoring end-to-end connectivity with IPv6"
outputs = ["Reveal"]
+++

# Restoring end-to-end connectivity with IPv6

Fixing what's been broken in our internet for 35 years

{{% note %}}
Welcome everybody to my first tech talk, my name is Tristan and I will be speaking about IPv6. This talk's main focus will be restoring end-to-end connectivity on the internet, a fundamental paradigm that has been broken for a very long time is mostIPv4 deployments around the world.
{{% /note %}}

---

## Agenda

1. **The basics of the internet and IPv4**
2. Problems caused by IPv4
3. The basics of IPv6
4. How IPv6 addresses those problems
5. Transitioning to IPv6
6. IPv6 in the wild today

{{% note %}}
I'm going to start by explaining how IPv4, IPv6 and the internet work in general. Then I'll talk about the pain points of IPv4 and how IPv6 addresses those. Finally, I'll talk about what others are doing with IPv6 right now and how you can get involved.

IP networking in general is a vast and expansive topic and it can get complicated really quickly so I'm deliberately going to skip over the nitty gritty implementation details of IP networking and give a simplified rundown of important takeaways.
{{% /note %}}

---

## A brief timeline

1981: IPv4 is released

1999: IPv6 is released

{{% note %}}
18 years later, we noticed there were improvements to be made to IPv4 and came ip with drafts for IPv6. Unfortunately, it took decades for v6 to gain mass adoption and it still isn't nowhere near the adoption level for v4
{{% /note %}}

---

#### What you should know about IPv4

{{% fragment %}}
Addresses are 32-bit numbers
{{% /fragment %}}

{{% fragment %}}
They range from 000.000.000.000 to 255.255.255.255
{{% /fragment %}}

{{% fragment %}}
There are a total of 4 294 967 286 available addresses
{{% /fragment %}}

{{% note %}}
I'm sure most of you are quite familiar with IPv4 addresses and addressing in general. It's quite likely that you haven't ever seen or used IPv6 and that's going to change today if that is indeed the case. IPv4 addresses are 32-bit integers and denoted using four octets from 0 to 255. I'll speak more about the why when we take a brief look at subnetting. This adds up to a total of 4 294 967 286 unique addresses in the IPv4 internet
{{% /note %}}

---

#### Subnetting

{{% fragment %}}
Divides address space between organizations
{{% /fragment %}}

{{% fragment %}}
Divisions based on the number of bits masked off the address
{{% /fragment %}}

{{% fragment %}}
Written in CIDR notation (/8, /22, /29)

![CIDR Notation](/img/cidr_notation.png)
{{% /fragment %}}

---

#### The structure of the internet

{{% fragment %}}
"The internet" isn't a singular entity
{{% /fragment %}}

{{% fragment %}}
The internet is actually a ton of small interconnected networks
{{% /fragment %}}

{{% fragment %}}
Network traffic travels end-to-end by crossing many different networks
{{% /fragment %}}

{{% fragment %}}
Routers peer to interconnect networks
{{% /fragment %}}

{{% fragment %}}
Traffic takes different routes
{{% /fragment %}}

{{% note %}}
It's common for people not to think much about the architecture of the internet and rather see it as a big cloud on the other side of the cable going to their service provider. It's actually a lot more complex and interesting than that.

The internet isn't a single entity but rather a giant internet-connected web of smaller networks. Network providers meet at internet exchange points (IXPs) in Meet-Me Rooms which are literally just rooms with cables everywhere up to the ceiling in which different internet service providers connect their routers to each other to form the global network we know as the internet.

Because there are so many interconnections, traffic on the internet can take many different paths, through different service providers, to get to the same destination. Generally, we optimize networks so the shortest and often fastest path is always chosen. Accessing a website from two different countries will generate two wildly different paths to the same destination.
{{% /note %}}

---

{{% section %}}

#### Physical view of the internet

![Submarine Cables](/img/submarine_cables.png)

{{% note %}}
If we look at this global network from the physical point of view, we can see that numerous submarine cables link up different continents of the world via many different paths to choose from.
{{% /note %}}

---

#### Logical view of the internet

![Submarine Cables](/img/bgp_simplified.png)

{{% note %}}
Diving deeper into the logical topology of these interconnections, it' even more spread out. Different ISPs use ASNs to identify their networks, which are the numbers in gray circles you are looking at. Every one of those ASNs has routers in it that advertise which IP addresses they can reach. Essentially, all of these routers speak BGP, the Border Gateway Protocol, to exchange routing information and tell each other about which networks they can reach. This is a process which constructs the global routing table. Routers form paths between each other to deliver internet traffic to the destination address as efficiently as possible.
{{% /note %}}

---

#### Logical view of the internet

![Submarine Cables](/img/bgp_complicated.jpg)

{{% note %}}
The previous illustration was for demonstration purposes only. This is what a manp of all of the ASNs in canada looks like. There are a lot of interconnections on the internet. You'll notice that there are big transit providers in the middle with hundreds or even thousands of connections. These big providers provide traffic to smaller service providers. For example, AS577 in the middle is Bell Canada.
{{% /note %}}


{{% /section %}}

---

### The global routing table

{{% fragment %}}
Contains everyone's subnets
{{% /fragment %}}

{{% fragment %}}
Used by routers to send traffic to destination
{{% /fragment %}}

{{% fragment %}}
Limited in size, routes are summarized
{{% /fragment %}}

{{% note %}}

The global routing table is a table of every route reachable on the internet. It is maintained in a decentralized fashion using BGP.

On august 12th 2014, the global IPv4 routing table surpassed 512K entries and caused issues with old Cisco routers. Cisco ended up cannibalizing TCAM space reserved for the IPv6 routing table to fix this for customers who did not upgrade to newer routers.

{{% /note %}}

---

## Agenda

1. The basics of the internet and IPv4
2. **Problems caused by IPv4**
3. The basics of IPv6
4. How IPv6 addresses those problems
5. Transitioning to IPv6
6. IPv6 in the wild today

{{% note %}}

I hope I haven't lost too many of you already but that's enough for a simplified overview of the internet.
Let's move on to the problems with IPv4, or more specifically, THE big problem.

{{% /note %}}

---

#### The major pain point of IPv4

## Address exhaustion

{{% fragment %}}
The world population is ~8 billion

There are at least that many devices on the internet
{{% /fragment %}}

{{% fragment %}}
There are ~4.3 billion addresses in IPv4

You see the problem...
{{% /fragment %}}

{{% note %}}

The world population is ~8 billion people, there are at least that many internet-connected devices on the planet. Just think about how many internet-connected devices you own plus all of the IoT stuff in your house.

You see the problem...

Back in 1994, the internet was already seeing widespread adoption and it became clear that 32 bits wasn't enough. Way too many devices were starting to become internet-connected and it didn't take long for people figure out we were going to run out of IP addresses if we needed to allocate one to every device.

{{% /note %}}

---

#### End-to-end connectivity

{{% fragment %}}
Every host can reach every other host
{{% /fragment %}}

{{% fragment %}}
In both directions
{{% /fragment %}}

{{% fragment %}}
Empowers peer-to-peer applications
{{% /fragment %}}

{{% note %}}

A quick note on end-to end connectivity. It's in the title of this talk but I haven't spoken about it.

In essence, what people mean by end-to-end connectivity is that both ends of a connection can reach each other. I can initiate a connection to someone else, and they can initiate a connection to me.

This is a fundamental principle which especially empowers peer-to-peer applications like WebRTC calling or BitTorrent file transfers.

{{% /note %}}

---

{{% section %}}

#### What we came up with

Unfortunately the answer wasn't "switch to v6"

## NAT

Network Address Translation

Reserve space for "private" addressing (RFC1918)

`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`

Use private addressing "inside" and translate to public addressing "outside"

{{% note %}}

Unfortunately the answer wasn't "switch to v6"

We came up with Network Address Translation, or NAT, to save IP address space by shoving computers into private ip ranges defined in RFC1918 and only give public addresses to servers that really need them.

{{% /note %}}

---

#### How does NAT work?

#### Source Nat + Masquerade

![Source NAT](img/source_nat.png)

{{% note %}}

A picture is worth a thousand words. Because a computer is using a private address, the server on the other end would have no idea how to send the response back to the computer since that ip address is not in the global routing table. With source NAT, the router masquerades the packet's source IP address with the router's own public IP address. The server then knows how to route the packet back to the router and the router unmasquerades the destination IP of the packet and replaces it with the PC's source address.

This kind of NAT breaks one of the fundamental principles the internet is built on, the end-to-end principle. As far as the server in this example is concerned, all of the hosts on the private network behind the NAT router are essentially the same host because they all share the same IP address. It is impossible for the server to directly connect back to a host, this connection only works one way.

{{% /note %}}

---

#### How does NAT work?

#### Destination NAT

![Destination NAT](img/destination_nat.png)

{{% note %}}

A picture is worth a thousand words.

This might be a bit easier to digest for some because this is what is widely referred to as "Port Forwarding" and I'm sure many of you have messed with this in your router's settings at some point to host a server of some kind on your home network.

It's impossible for outside clients to connect to servers hosted on your local network because they are hosted on private IP addresses. Because of that, we need a translation on the router that will map a port on the router's ip address to another one on the internal network.

This also means you can't host two services on the same port (for example multiple ssh servers or web servers) because you might only have one public ip address even if you have many private ones.

{{% /note %}}

{{% /section %}}

---

{{% section %}}

#### If that doesn't sound bad enough yet, don't worry because it gets worse

### Enter CG-NAT

Carrier-Grade NAT

Even internet service providers are running out of IPv4

They came up with a brilliant solution

Just stick another NAT between our customers and the internet

`10.64.0.0/10`

{{% note %}}

Eventually, internet service providers realized that even with NAT, they didn't have enough IP addresses to give one out to very single one of their customers' routers. I think you can see where this is going...

Carrier-Grade NAT is almost the same as the NAT found in your home router, except they stuck another layer of it after your home router, so you are effectively "Double-NATed" when behind a CG-NAT.

Ip addresses in the range 10.64.0.0/10 were reserved for private ip addresses service providers distribute to their customers.

{{% /note %}}

---

#### How does NAT work?

#### CG-NAT

![CG-NAT](img/cg_nat.svg)

{{% note %}}

This essentially means your router gets a private IP address instead of a public one and a second set of NAT + Masquerading after your router happens on the ISP's end which effectively means you can't host anything even if you port forward because you would need your ISP to also port-forward from their public IP to your router's private IP.

{{% /note %}}

{{% /section %}}

---

## Agenda

1. The basics of the internet and IPv4
2. Problems caused by IPv4
3. **The basics of IPv6**
4. How IPv6 addresses those problems
5. Transitioning to IPv6
6. IPv6 in the wild today

{{% note %}}

Now that I've gone on and on about all of the terrible hacks we've deployed to cope with IPv4's imminent threat of exhaustion, let's get a breath of fresh air and talk IPv6.

{{% /note %}}

---

#### IPv6 to the rescue!

{{% fragment %}}
Selling point: exponentially more addresses (340 Trillion!)
{{% /fragment %}}

{{% fragment %}}
128-bit addresses from `0000:0000:0000:0000:0000:0000:0000:0000` to `FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF`
{{% /fragment %}}

{{% fragment %}}
Multiple addresses per interface
{{% /fragment %}}

{{% fragment %}}
Simplified packet header
{{% /fragment %}}

{{% fragment %}}
Address autoconfiguration
{{% /fragment %}}

{{% fragment %}}
Duplicate address detection
{{% /fragment %}}

{{% fragment %}}
Much more
{{% /fragment %}}

{{% note %}}

IPv6's biggest selling point is that it touts a whopping 340 trillion IP addresses!

IPv6 addresses have 4 times as many bits as IPv4 addresses coming in at 128 bits.

The notation has changed a bit to cope with this, so they are now divided into 8 octets from 0 to 65535, but in hex to make things a bit more compact, so from 0 to ffff.

IPv6 is NOT just ipv4 with more addresses, there are a lot of extra goodies such as mandatory support for multiple addresses per interface, a simplified packet header for faster processing, address autoconfiguration, duplicate address detection and much more.

{{% /note %}}


---

#### IPv6 addresses

{{% fragment %}}
Can be shortened, `2607:00de:0045:0000:0000:0000:dead:00e1` -> `2607:de:45::dead:e1`
{{% /fragment %}}

{{% fragment %}}
Subnetting works the same way but with `/128` bits.
{{% /fragment %}}

{{% fragment %}}
Can be generated based on MAC address
{{% /fragment %}}

{{% fragment %}}
Can be randomized for privacy
{{% /fragment %}}

{{% fragment %}}
There's enough IPv6 for every device in the world
{{% /fragment %}}

{{% fragment %}}
Restore the end-to-end principle
{{% /fragment %}}

{{% note %}}

IPv6 addresses may look daunting at first because of their length, but rest assured they can bne shortened first of all, a patch of zeroes can be abbreviated with two colons and any leading zeroes can be omitted. You shouldn't be needing to remember v6 addresses anyway because DNS exists to address that problem.

Additionally, there are ways to autogenerate v6 addresses based on the interface mac address, they can be autogenerated to enhance privacy on the internet.

Most importantly, there's enough IPv6 for every internet-connected device which means that end-to-end connectivity can be achieved. Any computer on IPv6 can connect to any other computer in IPv6 as long as there is a routed path and appropriate firewall rules are in place.

{{% /note %}}

---

#### IPv6 Address Types

GUA: `2000::/3`, most like `0.0.0.0/0`

ULA: `fc00::/7`, most like `10.0.0.0/8`

Link-Local: `fe80::/10`, most like `169.254.0.0/16`

{{% note %}}

There are 3 different types of IP addresses you should know about in IPv6.

GUA, Globally Unique Addresses are the standard ip address everyone should be using in IPv6, these addresses can be routed on the internet and are most like any public ipv4 address.

ULA, Unique Local Addresses are non-routable IPv6 addresses which shouldn't need to be used except in some specific use case I might talk about if I have some time at the end. They are kind of like private addresses in IPv4 except they won't overlap between two networks.

Link-Local IP addresses are in a subnet available on every single interface in IPv6, they are used to communicate with any other device physically connected to the same link, such as between a computer and a router directly or between two computers with no router.

{{% /note %}}

---

{{% section %}}

#### SLAAC

StateLess Address Auto Configuration

Devices choose their own address

No need for a DHCP server keeping track of leases

Static Addressing

Privacy Addressing

Requires a `/64` or larger subnet

{{% note %}}

An interesting part of IPv6 is its ability to let devices auto-configure an IP address without the need for a DHCP server keeping track of leases on a router. This is called StateLess Address Auto Configuration. The router simply needs to advertise the IPv6 prefix (the leading bits of the address) and the device can choose its own address such as one derived from the mac address or randomly generated ones for privacy. Static addresses can also be configured.

Of course, DHCPv6 also exists and can be used to replace SLAAC or in conjunction with SLAAC to provide extra information

{{% /note %}}

---

#### SLAAC

#### EUI-64 Address

Predictable and static IP addresses

Available on every interface via link-local address

![EUI-64](/img/eui64.gif)

{{% note %}}

EUI-64 is a scheme which allows an IP address to be derived from a MAC address, very useful to predict what a device's IP address will be without doing any configuration.

The actual math is irrelevant to this talk, I just wanted to show a good visul explanation.

{{% /note %}}

---

#### SLAAC

#### Privacy Addresses

Randomly generated

Multiple addresses

Frequently rotated

```
    inet6 2001:470:b08b:51:7d95:1d02:b4da:a8c/64 scope global temporary dynamic
       valid_lft 520879sec preferred_lft 2027sec
    inet6 2001:470:b08b:51:f2d:3b3e:ba41:10b/64 scope global temporary dynamic
       valid_lft 434933sec preferred_lft 0sec
```

{{% note %}}

SLAAC can also be used to generate randomized addresses using the privacy extensions defined in RFC4941, which allows devices to generate multiple random ip addresses with short lifetimes and rotate them.

{{% /note %}}

{{% /section %}}

---

## Agenda

1. The basics of the internet and IPv4
2. Problems caused by IPv4
3. The basics of IPv6
4. **How IPv6 addresses those problems**
5. Transitioning to IPv6
6. IPv6 in the wild today

{{% note %}}

Now that I've covered the basics of IPv6 and the improvements it brings to IPv4, I'll quickly talk about how IPv6 solves the address exhaustion problem IPv4 has and how it restores end-to-end connectivity.

{{% /note %}}

---

#### Implications of GUA Addressing

{{% fragment %}}
Addresses for every device on your internal network
{{% /fragment %}}

{{% fragment %}}
We are STILL protected by firewalls!

Everyone can reach each other else given correctly configured firewall rules
{{% /fragment %}}

{{% fragment %}}
No more "public" and "private" ip addresses

Only "globally routable" ip addresses
{{% /fragment %}}

{{% fragment %}}
Multiple services can be hosted on the same port within an internal network
{{% /fragment %}}

{{% note %}}

When using IPv6 GUA addressing, suddenly every device on your internal network has one or many public ip addresses. To the uninitiated, this might sound scary but it shouldn't. We still have firewalls in place on our routers that prevent inbound connections to internal devices unless configured not to.

We can drop the terms "public" and "private" for ip addresses and think of them as "routable" and "non-routable" ip addresses.

Multiple services can be hosted on the same port within an internal network

{{% /note %}}


---

#### Common misconceptions about ipv6

{{% fragment %}}
IPv6 is not IPv4 with more address bits
{{% /fragment %}}

{{% fragment %}}
NATs do NOT provide any additional security
{{% /fragment %}}

{{% fragment %}}
Using GUA addresses does not expose your internal network
{{% /fragment %}}

{{% note %}}

just want to re-iterate this.

Additionally, just because an ip address is routable, doesn't mean it has to be routable. You can use a GUA prefix internally and never advertize it to the global table, to it can never be reached from the outside and can only be used for internal communication.

{{% /note %}}


---

## Agenda

1. The basics of the internet and IPv4
2. Problems caused by IPv4
3. The basics of IPv6
4. How IPv6 addresses those problems
5. **Transitioning to IPv6**
6. IPv6 in the wild today

{{% note %}}

Now that I've covered the basics of IPv6 and the improvements it brings to IPv4, I'll talk about strategies used to migrate to IPv6 form IPv4.

{{% /note %}}

---

#### Transitioning to IPv6

{{% fragment %}}
Dual-stack networks
{{% /fragment %}}

{{% fragment %}}
Single-stack IPv6 with tunnelled/translated IPv4
{{% /fragment %}}

{{% note %}}

There are two paths one can take, either dual-stack networking or native ipv6 with tunnelled ipv4

{{% /note %}}

---

#### Dual-stack networking

{{% fragment %}}
IPv4 and IPv6 in parallel
{{% /fragment %}}

{{% fragment %}}
Hosts have two different addresses
{{% /fragment %}}

{{% fragment %}}
IPv6 is preferred and IPv4 used when IPv6 is not available
{{% /fragment %}}

{{% fragment %}}
Simple to implement
{{% /fragment %}}

{{% fragment %}}
Good for home/office networks
{{% /fragment %}}

{{% note %}}

Dual-stack is the most straightforward and easy path. You just run both protocols in parallel and use IPv6 default and fallback to v4 for unsupported things.

{{% /note %}}

---

{{% section %}}

#### Single-stack v6 with NAT64 & DNS64

Internal network is IPv6 only

DNS64 synthesizes A responses into AAAA

NAT64 translates between v4/v6 at the network border

Harder to implement

Breaks certain protocols

Good for server networks

{{% note %}}

DNS64 and NAT64 allow a network to run on ipv6 only but allow applications that still rely on ipv4 connectivity to access ipv4 resources on the network using DNS64, a DNS scheme that translates IPv4 DNS answers into IPv6 ones and a special type of nat, NAT64 that translates between IPv4 and IPv6. Clients don't have an IPv4 address at all.

{{% /note %}}

---

#### NAT64 & DNS64 Illustrated

`0.0.0.0/0 -> 64:ff9b::/96`

![NAT64 & DNS64](/img/nat64_dns64.png)

{{% /section %}}

{{% note %}}

Quick illustration, the prefix 64:ff9b::/96 is reserved and used to represent the entire ipv4 address space in ipv6. We can do this because 32 bits easily fits inside 128 bits. This breaks certain protocol which rely on hardcoded ipv4 literals because the host will have no IPv4 address and thus no way to speak IPv4.

{{% /note %}}

---

{{% section %}}

#### 464XLAT - A mix of both

Native IPv6 connectivity

IPv4 is transported over IPv6

End device still has IPv4 support (no protocol breakage)

IPv4 is NAT/CG-NATed

{{% note %}}

464XLAT is similar to NAT64 and DNS64 but it also gives the device a NATed IPv4 address to address some breakage. This scheme is mostly used in mobile networks and is well supported in Android.

{{% /note %}}

---

#### 464XLAT Illustrated

![464XLAT](/img/464XLAT.png)

{{% /section %}}

{{% note %}}

You don't really need to know all of this technical stuff but it's just a nice illustration showing that 464XLAT provides native ipv6 and NATed ipv4, over IPv6.

{{% /note %}}

---

## Agenda

1. The basics of the internet and IPv4
2. Problems caused by IPv4
3. The basics of IPv6
4. How IPv6 addresses those problems
5. Transitioning to IPv6
6. **IPv6 in the wild today**

{{% note %}}

To conclude this talk, I just wanna briefly talk about what other people are doing with IPv6 in the wild and what you can do to hop on the bandwagon.

{{% /note %}}

---

#### IPv6 deployment statistics by Google

![IPv6 Adoption Chart](/img/ipv6_adoption.png)

{{% note %}}

Google tracks the amount of IPv6 connections to their services vs IPv4 connections and the number gets better every year.

The biggest IPv6 deployments worldwide are mobile phone networks. Many carriers around the world have IPv6-only networks. Residential internet providers don't have a good track record but that has been changing a little recently, residential providers are getting better at supporting IPv6 every year.

The biggest lack of IPv6 connectivity is attributed to corporate networks stuck on IPv4 with no V6 access in sight.

{{% /note %}}

---

#### Large tech companies are going IPv6-only in the datacenter

## Facebook

## Linkedin

{{% note %}}

Large tech companies like Facebook and Linkedin have made strategic bets on IPv6 in the datacenter because it greatly simplifies IPAM (ip address management) when being able to think about global addresses and not NATed IPv4. Linkedin and Facebook both have great posts on their engineering blogs talking about how the did it.

{{% /note %}}

---

#### IPv6 is up to 40% faster than IPv4

{{% fragment %}}
Measured by Facebook end-to-end

Over cellular and wifi networks
{{% /fragment %}}

{{% fragment %}}
Attributed to the lack of middleboxes and NATs
{{% /fragment %}}

{{% note %}}

According to studies facebook made in 2015 using their mobile application, the noticed their app loading from 20 to 40% faster on IPv6 and attributed this change to the lack of multiple layers on NAT (because mobile networks have historically been the heaviest users of NAT) slowing down connection times.

{{% /note %}}

---

#### IPv6 deployments in Canada

Mobile networks: Rogers(Fido), Bell(Virgin)

Residential networks: Rogers, Telus, TekSavvy (DSL), Videotron Helix

Tunnels: Videotron 6rd, Hurricane Electric (https://tunnelbroker.net)

👎👎 Videotron Cable, Bell Fibe, Telus Mobility 👎👎

{{% note %}}

I used to be on Rogers and I used to have Native IPv6 and Native IPv4. This changed when Android 9.0 came out on my phone and I ended up with 464XLAT translation, so Native IPv6 and NATed IPv4 transported over IPv6. I know Bell and Telus have similar networks. Big thumbs down for Videotron and Fizz for IPv6 on mobile, they only provide CG-NATed IPv4.

{{% /note %}}

---

## Thanks for listening
#### Questions Welcome

![Anti RFC1918](/img/anti_rfc1918_aktion_small.png)
