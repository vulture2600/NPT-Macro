# NPT-Macro
Macro written for the HAAS CNC control to cut NPT threads of any size using a single point threading tool.
Steve.a.mccluskey@gmail.com


I wrote this macro a few years ago. It is designed to cut tapered threads of any diameter and any thread pitch with a constant taper of 3/4" per foot, or 1/16th of an inch per inch. For anyone who knows about cutting NPT threads with a single point tool, it its not easy to program, especially with cutter compensation.

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

Where do we get the parameters?  The way I approach this problem is to visualize the smallest measurable point of a tapered object to fit into a tapered hole at a specified depth. When cutting female NPT threads, we generally have a thread gage or a threaded pipe that needs to screw into the finished thread to a specified depth, most often found in the Machinery's Handbook. For example, the 2-1/2 x 8 TPI NPT thread gage we have at the shop shows a tip-to-flat insertion dept of .68". The Z depth I use is a full thread pitch deeper than that, which is pretty common practice. My calculated Z depth will be .68 + (1/8TPI) which is equal to .80  The first full thread's major diameter measures 2.80.  For the cutter diameter, this macro works with diameter mode, so the value has to be equal or greater to the diameter of the cutter. This macro also adds together the diameter and wear values in the control so you can use either or both. For example, D22 diameter might be 1.000 as a nominal value and the wear value might be -.015 for an actual diameter of .985. Any machinist should know to start with a cutter diameter value larger than nominal and creep in on the final dimension of the finished feature size to be cut. In this case
