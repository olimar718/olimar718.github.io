---
title: "Wifi antenna advanced : How to turn a old laptop into a Wifi antenna"
---
## The problem

I recently moved to a new apartment, where I don't have to pay for electricity, water or internet access. Great right ? Who likes to pay ? Anyway, of course, the "free" internet access is used by multiple tenant, so it is a wireless access point, and as it is outside the apartment, there is no way to run a good old RJ-45 cable to my desktop PC. 

Of course my desktop PC's motherboard doesn't have bundled wifi (honestly now I regret it, I should have bought a motherboard with bundled wifi). So how do I get internet access to my desktop PC ?

## Possible solutions

Of course the first and most obvious solution is to buy a wifi card. That would be a very good solution but of course you would have to pay for it which is not really what we want to do. While you can find theses cards for as low as 10 €, you probably have to go to 40-50 € to get a decent one.

| ![Famous French website topachat for a wifi card](images/wifi_card_listing_topachat.png) | 
|:--:| 
| *A low end wifi card that you probably should not buy* |

The free solution that most people would think about first is to use a feature that's present in most modern android phone. This feature, called USB Tethering, allows you to use your phone as router + NAT for a computer via USB. In simple words it allows to share the internet connection (be it wifi or mobile data) of the phone to the computer. 

This solution is extremely easy to use and completely plug and play on Windows and on modern linux distribution for fedora. You plug your phone to the computer, turn on the option, et voilà, it works.

| ![USB Tethering option on Android](https://cdn-0.androidphone.fr/wp-content/uploads/2020/10/partage-de-connexion-usb-android-2-768x661.jpg?ezimgfmt=ng:webp/ngcb21) | 
|:--:| 
| *USB Tethering option on Android (screenshot not from me)* |

So I was actually using this solution with one of the phone I had lying around for a while but it wasn't very satisfying in the long run. Phone are of course not meant to be used 24/7 for usb tethering, and so, in my experience it wasn't working so great for 2 reasons:

- First, since I was using an older smartphone (a very unique Asus Zenfone 2, that actually uses an intel x86 processor instead of a ARM as you would find in almost every smartphone) the connection was using usb 2.0. This meant that the throughput, while acceptable, is fairly limited, compared to theoretical maximum of whatever version of WiFi the access point is using. Similarly I experienced latency problems when talking to people on discord, and when playing online games such as rocket league.
- Secondly and most importantly, the connection would randomly stop working and essentially, usb tethering would stop working spontaneously.Toggling it off and back on would fix it but that's still very annoying in the middle of a discord call. The bigger problem here is that I also use my desktop PC as a mining rig (whit a single MSI gtx 1060 6gb gaming X), and I'm often away from home (or sleeping), therefore unable to manually toggle usb tethering off and back on to make it work again. This would result in downtime for my mining rig and a (admittedly small) loss of money.

Therefore I needed a better solution

## The solution : Using a Laptop as a WiFi antenna 

Of course "WiFi antenna" is a massive misnomer here, as in fact the LapTop behaves much like a android phone used with usb tethering.

In my case I used a decade old HP EliteBook 8440p, but any laptop with wifi and a RJ45 port should work. In spite of its age, this laptop actually has a very good wifi module that even supports 5GHZ WiFi. That module is not even soldered to the motherboard, and can be changed easily, as it is the case with most laptop from that era.

| ![mini pci wifi module from intel](https://m.media-amazon.com/images/I/717CAn5q8DS._AC_SL1246_.jpg) | 
|:--:| 
| *A mini pci wifi module from intel, from a amazon listing, the one in the laptop looks exactly like that* |

Now I will go over what you need to do to turn a laptop into a RJ45 tethering device

### Operating System setup

While it might be possible to do everything on windows, I used [alma Linux](https://almalinux.org/fr/), which is basically the continuation of CentOs, which was the free fork of redHat enterprise Linux, a commercial Linux distribution (I know, it's a lot more complicated than it should be).

Anyway the choice of distribution really should not matter, as long as you stick to the famous ones. You can also install a graphical environment but you won't need it so don't bother.

### First step : Connection to WiFi in command line

### Second step : enable the linux built in router (Aka ip forwarding)

### Third step : plug in the RJ45 on both end, configure the network interface

### Fourth step : enable IP masquerade (NAT)

### Fifth step (optional): Install a DHCP server and configure it

## Conclusion 

| ![A picture of the setup in production](images/wifi_antenna_advanced_in_production.jpg) | 
|:--:| 
| *A picture of the setup in production* |

{% include footer.md %}