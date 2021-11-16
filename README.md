# NPT-Macro
Macro written for the HAAS CNC control to cut NPT threads of any size using a single point threading tool.

Steve.a.mccluskey@gmail.com


I wrote this macro a few years ago. It is designed to cut tapered threads of any diameter and any thread pitch with a constant taper of 3/4" per foot, or 1/16" per inch. For anyone who knows about cutting NPT threads with a single point tool, it its not easy to program, especially with cutter compensation.

I developed this for several reasons:
1) NPT threads are a true spiral and impossible to program at the machine without CAD/CAM software.
2) Concentric helixes of one pitch each with increasing diameters work but are really difficult to calculate at the machine, and in my opinion, a poor approximation of a true spiral.
3) Cutter compensation add another layer of difficulty and basically impossible to impliment.
4) I wanted something that will cut a constant taper of any thread pitch or diameter because some there are times when making something non-standard is required.
5) I wanted a closer approximation to a true spiral. This macro cuts 90 degree arcs in quadrants instead of a full circle, and steps out as neccesary between each arc. I'm sure there is a way to divide each cut into an almost infinite amount of arcs but 4 was quite a bit better than 1 per pitch.

Use as follows: drill, bore, or interpolate the tap drill size according to any standard tap drill chart. Chamfer the hole as neccesary. Position the single point tool .3 above
the center of the hole.
I use a custom G code for this and have assigned it 152.
Syntax: G152 I J D E F Z.

For example, to cut the threads for a 2-1/2 x 8 TPI NPT Thread, you would enter
G152 I2.81 J8 Z-.80 E50. F40 D22
I: Major diameter of the smallest thread near the tip of the pipe or gage to be used.
J: Thread pitch in threads per inch.
D: Diameter of the single point threadmill to be used.
E: Plunge rate to specified Z start depth
F: Feed rate for threading.
Z: Start depth.

If E is not specified, plunge rate will be the same as the cutting feed rate.

Where do we get the parameters?  The way I approach this problem is to visualize the smallest measurable point of a tapered object to fit into a tapered hole at a specified depth. When cutting female NPT threads, we generally have a thread gage or a threaded pipe that needs to screw into the finished thread to a specified depth, most often found in the Machinery's Handbook. For example, the 2-1/2 x 8 TPI NPT thread gage we have at the shop shows a tip-to-flat insertion dept of .68". The Z depth I use is a full thread pitch deeper than that, which is pretty common practice. My calculated Z depth will be .68 + (1/8TPI) which is equal to .80  The first full thread's major diameter measures 2.80.  For the cutter diameter, this macro works with diameter mode, so the value has to be equal or greater to the diameter of the cutter. This macro also adds together the diameter and wear values in the control so you can use either or both. For example, D22 diameter might be 1.000 as a nominal value and the wear value might be -.015 for an actual diameter of .985. Any machinist should know to start with a cutter diameter value larger than nominal and creep in on the final dimension of the finished feature size to be cut.

This macro is designed to specify any diameter and any thread pitch. Why would I make this possible when NPT threads are standard? I could have easily written it so that I is the nominal pipe size and it figures out the rest using common values, but I figured a universal approach is better over all. There have been several times another machinist brings me a threaded object and there was no gage or way to accurately measure it, so this allows you to make one thing fit into another, what ever they happen to be.

The macro checks to see if variables are provided and in range. For example, Z must be negative. Single block is suppressed early on in the program so you don't have to single block through all of the parameter checks. Single block is restored right at the first place machine movement is called.

Also worth noting is that G41/G42 cutter comp is NOT used. However, the diameter of the cutter specified in the cutter comp list is absolutely used. The radius of the first cut is calculated as followed: (Diameter of first thread / 2) - (cutter diameter / 2). Subsequent arcs continuuously increase as neccessary. The marco calculates the number of threads required to clear the top of the hole at Z0 before returning to Z.3 and returning to the previous program. Any Diameter number can be used, even on the same tool. This is so that you can compensate different holes seperately. You can also just change the "I" value accordingly to make the threads bigger or smaller.


Lastly, use at your own risk. Any machinist should know how to set up and proof out a program. My rule of thumb is to always add 1.00 to the tool lenght value or raise my work offset one inch above the part so I can run the program with minimal chance of crashing and to verify that the tool is following the hole sides properly. If the cutter goes way past the hole, you did something wrong. Its better to run this safely then to break a tool and/or destroy a part.

Any questions feel free to email me!
