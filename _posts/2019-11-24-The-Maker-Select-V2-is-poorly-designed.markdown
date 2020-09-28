---
layout: single
title:  "The Maker Select V2 is poorly designed"
date:   2019-11-24 19:00:00 -0500
categories: 3D-printing DIY
---

I have been working with 3D printers for nearly 10 years. I started back in college during my third year final when my team tackled designing and building a 3D printer from scratch. Following this project, I picked up a build it yourself Prusa I3 kit and assembled the printer over the course of a month with one of my friends. This printer was good, but it was a cheap kit. I wanted something more. 

My girlfriend was listening and teamed up with my family to pitch in on a Monoprice Maker Select V2. This was a cartesian style printer modeled after and very similar to the Prusa I3. It was a great design at first glance, with a rigid metal frame and great quality parts. 

![Maker Select V2](/assets/images/3D-printing/maker-select-v2.jpg)

Finally, I picked up the FLSun QQ, which was a Delta style printer with a rigid metal body. 

![FLSun QQ](/assets/images/3D-printing/FLSunQQ.jpg)

These new printers provided me years worth of prints with very few issues. You must be wondering why the Maker Select V2 is so poorly designed after everything I just said. Well, a few years into using the printer (roughly two years ago), it started to smell like fire and the hot end stopped heating up. It was time to bust it open and find out what is wrong with it. 

With my experience in Electromechanical Engineering, I felt at home cracking open the case, and testing the wiring. My first step was to determine where the printer is failing. In order for the hot end to work, it needs to complete the circuit between itself and the board. If the connection at the hot end or at the board is broken, it obviously wont work. In addition to this, if the cable is broken at any point in between, you will have the same result. I powered on the printer and measured the voltage at the hot end with my multimeter... zero. This printer uses 12V to heat the hot end, so this was not unexpected. It would be difficult to test the cable somewhere in between, so I moved onto the board. This is where my mind was blown.

The issue did not eneed to be tested with my multimeter, for it was obvious. The connector was melted and blackened from the heat. Unfortunately I do not have the photos any more, but it looked like a drip of honey, but pitch black. I did some research and determined the stock board connector for the hot end was way under rated for the current the hot end used. The hot end required about 20-25A from what I recall, and the stock connector was some cheap unlabeled chinese connector that turned into tar from the load. I picked up some bulky XT60 60A connectors which were instead well over rated and replaced the hot end on the board. Success! The hot end was working again.

Despite the repair, I was shocked that a company producing 3D printers would make such a mistake. This is without a doubt the highest current component on the 3D printer. If I was in charge of designing the printer and I could make sure of one single thing, it would be to rate the connectors and cables correctly. 

So anyways, the printer was back in working order and I felt a lot more secure using it - that is until a few months ago when the hot bed also died. I immediately knew what the problem was. It was obviously the hot bed board connector which was the same as the hot end connector. The sad thing about this was that the hot bed draws far less current than the hot end as it doesn't need to maintain as high of a temperature. Despite this, this garbage connector still failed. 

Due to the lower current, it didn't melt like tar and it was visually impossible to determine any fault. Instead, I needed to use the multimeter. I started at the hot bed again, and found that it was getting 0V. I moved down the wire and to the board connector. I tested this and found this was also getting 0V. The two possibilities are: 
    
1. The connector is garbage and failed.
2. The board itself failed. 

This was not easy to test with the tools and experience I had. I opted to bust out the ol' XT60 connectors and solder the bad boys on. This process was pretty difficult as these connectors were much wider than the stock connectors and the fit was extremely tight. Still, I managed to solder it on and fired up the printer. Once again, success!

Below, you can see the difference between the connectors:

![XT60](/assets/images/3D-printing/XT60.jpg)

On the left, you can see the hard to miss XT60 connectors, which have the thickest wires running to them. To the right of those, you can see the cheap unlabeled chinese connectors. Normally connectors have some kind of identifier on them, but these do not, save for a label indicating what is plugged into them. These are obviously unfit for a heavy load. 

So although the rest of the printer is well built, the guts of this thing are not designed correctly. This kind of fault could have caused a fire, and I am in disbelief that this thing was mass produced and shipped to thousands of consumers. At the end of the day, my take away from this is that until 3D printers are better regulated and better meet the electrical code, the safest thing you can do is crack the printer open to have a look at the board. If any heavy load connectors are unlabeled chinese components, consider calculating the load yourself and replace the pieces accordingly.
