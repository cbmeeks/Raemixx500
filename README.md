# Rämixx500
Rämixx500 is an Open Hardware remake of the Commodore Amiga 500+ mainboard, revision 8A.1.

![Board](https://raw.githubusercontent.com/SukkoPera/Raemixx500/master/img/render-top.png)

## Summary
Many Amiga 500+ computers are suffering an early death because of the built-in barrel battery that powers their internal real-time clocks. Such batteries have long exceeded their planned lives and in many cases have started to leak alkaline liquids over the mainboard, corroding copper traces and destroying components.

This damage can sometimes be repaired trivially, but many times it requires a lot of time and effort. Sometimes it adds up to other damage occurred over time and so it would just be better to have a new board built with new components to move the few critical chips over. Amiga mainboards haven't been produced for the last 30 years, but they are relatively simple by today's standards, thus an amateur project to make new ones was started.

There are other projects like this one out there, but none of them is Open Source and none of them comes with both schematics and board. This is a big advantage, since anyone can modify the board and make new improved versions, as long as they release their modifications it with the same license. I have come up with [some ideas for improvements](https://github.com/SukkoPera/Raemixx500/issues/14), feel free to help :).

Now the bad news: **THIS BOARD IS UNTESTED!!! IT MIGHT NOT WORK AT ALL!** It might kill your cat or burn your house down. You have been warned.

While this might sound deceiving at first, **it is actually an opportunity to showcase the full potential of Open Hardware**: download the project files, inspect them and [report any errors](https://github.com/SukkoPera/Raemixx500/issues) you find! This way we can all work together to get a perfect board.

## Differences from original
The initial objective was to come up with a new mainboard as similar to the original one as possible, while including minor modifications that would improve its usability. First the schematics were drawn from scratch in KiCad and then the board was routed, staying close to the original layout.

This mainboard was designed with *reasonable* - not *maniacal* - accuracy to the original design. Most care was taken in the positioning of components that ought to be in a certain position (i.e.: screw holes and I/O connectors), to ensure drop-in replaceability. Other components and tracks are "more or less" there, but as the board was wholly laid out from scratch by hand, don't expect sub-millimeter accuracy.

Following is a list of deliberate changes with respect to the original layout of the A500+ rev.8A.1 board:
- The footprints for all DIP chips use "long pads". This makes them easier to solder and more solid to the board should you need to rework them. This forced a few tracks running very close to the original pads to be slightly offset away.
- C99 was added to allow for the correct usage of 318069-10/11 Agnus chips. Leave unpopulated for others (only 8375 will work).
- The power connector footprint was altered to either accept the original connector, a DIN-6 or a DIN-8. I actually recommend the latter, as it uses more than one pin for the +5V and +12V rails, allowing for more current.
- The floppy connector footprint was changed to that of a full IDC connector.
- A simple floppy drive switcher was integrated, just below the floppy connector. If you want to use it, cut the marked tracks under J90, solder a pin header and use jumpers/switches to switch.
- A simple Kickstart switcher was integrated as well, which supports up to 8x256Kb ROM images. If you want to use it, cut the marked tracks under J91/92/93, solder some pin headers and use jumpers/switches to switch.
- The barrel battery was replaced with a BS-7 battery holder for a normal (non-rechargeable) CR2032 battery. Consequently, R913 has been replaced with a diode (labeled D913) and a couple of tracks needed some displacement.
- The need to solder D912 to a leg of the former R913 has been removed. Just solder it in its place.
- The RCA jacks for the audio and composite video outputs have been replaced with some that can actually be found nowadays (i.e.: those that were used on A600/A1200). This resulted in relocating R409 (whose original position seemed somehow improvised anyway...).
- The above allowed a couple of tracks to be added so that the left and right audio channels will be somehow mixed whenever a single output jack is connected. This was lifted from the A600.
- The silkscreen for some components does not match the original one. I used the built-in KiCad footprints as-is, when available.
- The silkscreen and pitch of C303, C304 and C306 have been made smaller so that they don't overlap.
- Speaking about the silkscreen, I have been quite liberal with it. I have used the default KiCad font and I did not follow the original label placement at all costs. I did this since today's technology can give us a bit more resolution in silkscreen printing, and I think that is worth using for the sake of clarity. Some ground stitching vias were slightly offset to make up space for labels.
- The vias inside the pads of JP10A and JP11 have been slightly offset so that they are outside the pads.
- The ground fill is autogenerated by KiCad, so it won't match the original exactly.
- Probably there's something more I've forgotten.

## Assembly and Installation
Again: **PLEASE NOTE THAT THIS IS UNTESTED!!! IT MIGHT NOT WORK AT ALL!**

You will probably want to install all new components on this. Most passives should be easy to find, except for the axial caps and EMI filters.

The connectors are all on the market, except for the Video and Floppy ones, which are non-standard DB-23. You will need to recover these from a failed board. The same applies for the power connector, but the board should also accept cheaper DIN-6 or DIN-8 connectors. Make sure to wire it properly (Info soon). Oh, I have no idea if the line filter can be bought new, so you'd better recover that, too.

Other things you will need to recover are all the custom Commodore ICs, of course. Make sure you are using an 8375 Agnus, and populate C99 accordingly.

CPUs can be found second-hand cheaply. Every serious electronics shop should have all the 7400-series chips.

I would suggest using sockets for all ICs. Get new good-quality ones. 48-pin are hard to obtain, but you can easily replace them with two 24-pin side by side. Be careful with the RAM chips: if you socket them, they will probably be too tall for the keyboard to fit properly.

Probably you will also need to recover the original quartz, as it has an uncommon frequency (PAL: 28.37516 MHz, NTSC: 28.63636 MHz), but it seems to be available from some Chinese sources. You can also try replacing it with a [DFO](https://nfggames.com/forum2/index.php?topic=5744.0): this is untested, but with a properly-programmed one you should even be able to support both PAL and NTSC Agnus chips with the flick of a switch.

You can recover the original Video Hybrid, or you can [build a new one](https://github.com/SukkoPera/OpenAmigaVideoHybrid).

The solder jumpers should all be preset with the most common value (Detailed info on these will be added soon). You will just need to take care of JP4 if you decide to install only 512k chip RAM: in this case DO NOT install U32 and put a blob of solder on all the three pads of both JP4A and B.

If you use the standard MSM6242B Real-Time Clock, you will need to calibrate the clock frequency through the TC9 variable cap, which requires the use of an oscilloscope. If you decide to recycle it from an old board, try not to move it during the desoldering. You'd better mark its original position with a marker before removing it. In alternative you can use an Epson RTC62421 or RTC72421. In this case, do not install Y9, C911, TC9.

If you are not interested in the Real-Time Clock at all, you can skip the following components: R911, R914, D911, D912, D913, C9, C911, C913, U9, BT9, Y9, TC9. You should also put a blob of solder on JP9 so that the system will pick up the one that might be present on a card inserted in the trapdoor slot (in this case you can also skip R916).

The battery holder is called BS-7 and is very easy to find. Other ones will probably fit just as nicely. Make sure to use a NON-rechargeable battery.

Good luck! ;)

## Releases
If you want to get this board produced, you are recommended to get [the latest release](https://github.com/SukkoPera/Raemixx500/releases) rather than the current git version, as the latter might be under development and is not guaranteed to be working.

Every release is accompanied by its Bill Of Materials (BOM) file and any relevant notes about it, which you are recommended to read carefully.

**I am not providing ready-to-use gerber files**. If all you want is **to get boards made, I would really appreciate if you did so [in a way that supports the project](#support-the-project)**.

## License
*(I am not sure I can claim any copyright on this, as the actual schematics this is based on belong to Commodore (or whoever has that right now, definitely not me). So the claim below is going to be more of a declaration of intent, in the sense that I would like that everything that uses my work stays open and free.)*

The Rämixx500 documentation, including the design itself, is copyright &copy; SukkoPera 2019-2020.

Rämixx500 is Open Hardware licensed under the [CERN OHL v. 1.2](http://ohwr.org/cernohl).

You may redistribute and modify this documentation under the terms of the CERN OHL v.1.2. This documentation is distributed *as is* and WITHOUT ANY EXPRESS OR IMPLIED WARRANTIES whatsoever with respect to its functionality, operability or use, including, without limitation, any implied warranties OF MERCHANTABILITY, SATISFACTORY QUALITY, FITNESS FOR A PARTICULAR PURPOSE or infringement. We expressly disclaim any liability whatsoever for any direct, indirect, consequential, incidental or special damages, including, without limitation, lost revenues, lost profits, losses resulting from business interruption or loss of data, regardless of the form of action or legal theory under which the liability may be asserted, even if advised of the possibility or likelihood of such damages.

A copy of the full license is included in file [LICENSE.pdf](LICENSE.pdf), please refer to it for applicable conditions. In order to properly deal with its terms, please see file [LICENSE_HOWTO.pdf](LICENSE_HOWTO.pdf).

The contact points for information about manufactured Products (see section 4.2) are listed in file [PRODUCT.md](PRODUCT.md).

Any modifications made by Licensees (see section 3.4.b) shall be recorded in file [CHANGES.md](CHANGES.md).

The Documentation Location of the original project is https://github.com/SukkoPera/Raemixx500/.

## Selling the board
**The license allows you to sell these boards**. I have nothing against that **as long as you do so at an "ethical" price**. I understand you want to make some money, but please note that you are getting this all for free. You had no development costs and you invested no time in this project. I did, and my desire is to allow everybody to have a board at a REASONABLE price, so please keep your margin low. I estimate that **having a few of these boards produced costs about 20€ per board** (with ENIG, gold plating and beveling the edge connector, don't cheat on that!), so let me add the following restriction: **if this board is sold at more than 30€ (or equivalent in your currency), 25% of your earning MUST be donated to a LEGITIMATE charity** of some kind, like curing cancer for example. This amount has been set in April 2020 and will be updated, should the value of the Euro vary significantly.

Also, please **do not remove the credits, URL and license statement**. There is **no reason do so**, you have the right to sell this board, there is no need to pretend you got it somewhere else. Ironically, If you removed those, you'd lose that right as you'd be violating the license terms.

## Support the Project
If you want to support the project, you can order the boards from PCBWay through this link:

[![PCB from PCBWay](https://www.pcbway.com/project/img/images/frompcbway.png)](https://www.pcbway.com/project/shareproject/Raemixx500_1.html)

You get my gratitude and cheap, professionally-made and good quality PCBs, I get some credit that will help with this and [other projects](https://www.pcbway.com/project/member/shareproject/?bmbid=41100). You won't even have to worry about the various PCB options, it's all pre-configured for you!

Also, if you still have to register, [you can use this link](https://www.pcbway.com/setinvite.aspx?inviteid=41100) to get some bonus initial credit (and yield me some more).

You can also buy me a coffee if you want:

<a href='https://ko-fi.com/L3L0U18L' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi2.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

## Get Help
If you need help or have any questions or suggestions, you can join `#OpenRetroWorks` on FreeNode through your favorite IRC client or [the webchat](https://webchat.freenode.net/), or [the official Telegram group](https://t.me/joinchat/HUHdWBC9J9JnYIrvTYfZmg).

## Thanks
- Commodore, for making the coolest machine ever.
- [Amiga PCB Explorer](http://amigapcb.org), a fundamental tool to follow the original track placement.
- [amigawiki](https://www.amigawiki.org/doku.php?id=en:service:schematics), mainly for the schematics but also for the whole lot of information they provide.
- majinga for helping with the measuring.
- [Walter](http://smisioto.no-ip.org/elettronica/kicad/kicad-en.htm) for some of the footprints/3D models used.
- [DiagROM](http://www.diagrom.com/) for inspiring the selling clause.
- [Jason Warnes](https://www.everythingamiga.com/2019/10/alternative-to-amiga-500-600-1200-power-connector.html) for information about the original power connector and how to replace it with a DIN-8.
- [Szabó Zoltán](https://twitter.com/DeGenTd) for designing the board logo.
- [Workshopshed](https://twitter.com/Workshopshed) for the 3D models of the EMI filters.
- [StormTrooper](https://github.com/StormTrooper/Commodore-Plus4) for a few other 3D models, taken from his Plus/4 remake.
