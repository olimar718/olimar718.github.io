# Disabling processor core on a HP EliteBook Folio 1040 G3 to avoid random (and frequent) crashes
## The 1040 g3 : a great, but not very upgradeable laptop
[The EliteBook 1040 g3](https://support.hp.com/doc-images/962/c05262496.jpg)
I got my hand on this laptop at the end of my internship at Kayentis (alongside many others laptop, they basically gave me the whole warehouse of used laptop that could not be used again). This was by far the best one of the lot, but it was given to me because it was defective, so I had to fix it if I wanted to use it. 
This Laptop is a significant upgrade over the slightly older HP EliteBook 840 G1 that I was using before (and also got for free, trough my mom's job).
It is a very modern style of entreprise Office laptop. The device is extremely thin (Apple MacBook level thin) the touchpad does not have physical cliquable button (instead the whole touchpad is cliquable, which is not something I particularly like), it has a very good backlit keyboard, a much more respectable battery life than any laptop I had before  (like 7 hours + depending on the usage), as well as a much better screen and the ram and CPU are soldered to the motherboard (which of course is anoying for ram upgrades). Putting aside the soldered ram and the touchpad it is essentialy the type of laptop I like the most.

## The problem
The laptop would randomly crash to a blue screen under Windows, to a complete system freeze under Linux. Sometime, the device would not even boot sucessfully (it would crash in the middle of booting). Of course a full reinstall of Windows or Linux (Fedora 34) did not fix the problem either. The crashes were very frequent and would also happen in the middle of the multiple reinstals I tried.

## Possible solutions
Of course a motherboard replacement would certainly fix the problem entirely, but this option is very expensive (at least 150 €).
Another solution would be to put the motherboard in a oven, at a temperature of over 120 degree celcius, in an attemp to resolder possibly damaged solder point. In my opinion, for this specific problem it had a very low chance of sucess and could actually kill the motherboard entirely. But this actually a serious type of solution that works well for some type of electronics, see the relevant [Linus tech Tips video](https://www.youtube.com/watch?v=8Xanr4jkmEc) on the subject
The first thing to try is then to mess around with bios settings. The first thing I tought of was to disable intel
