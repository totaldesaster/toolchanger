# Low-Cost Open Source Toolchanging mechanism for 3D printers

Magnetic tool changer project, inspired by E3D and Jubilee tool changing printers and designed for low cost and wide compatibility. My goal is to make toolchanging accessible without owning four figure machines.

# How does it work?
![image](https://user-images.githubusercontent.com/112611063/208245435-d338f48e-3ce6-444d-a6e6-09b0cfb5a90c.png)
![image](https://user-images.githubusercontent.com/112611063/208245552-76d45640-5981-4120-9544-c5b3c500ecd8.png)
![image](https://user-images.githubusercontent.com/112611063/208245656-bfdf631d-43b8-4c83-bfc3-9f12c80852b1.png)

Both the tools and the toolhead have a circle of neodymium magnets with alternating north/south up polarities. Since opposing poles attract and same poles repel, the machine can switch between pulling and pushing on the tool by aligning the opposing or same poles of the magnets. This is done by a geared stepper motor on the toolhead which rotates the magnet circle (lockplate) - marked red and blue for north and south up in the first picture. Steel balls on the head and L profiles on the tool form a Maxwell coupling which ensures repeatable connections by having the tool contact the head in exactly 6 points - eliminating all 6 degrees of freedom, but not overconstraining the assembly, so there is theoretically exactly one position the tool can be in. The Klipper firmware is capable of controlling such a setup using the manual stepper feature and custom gcode macros. Similar features might exist in other firmwares as well.

# How do I use it?

Klipper code, CAD files and assembly drawings are documented on here. You'll need to design a carriage and dock base for your printer (please share with the community if you do!) and attach the components to it. Be aware that all of this is very experimental - you're welcome to test it and make any changes you want, but please note that this is not a finished product. I'm not responsible for any failures that may occur.

The 28BYJ-48 stepper motor can be driven with a normal stepstick with some minor modifications, you only need some basic tools. Follow the guide at https://www.youtube.com/watch?v=kq04-ReW1xY and wire it to a free stepper driver. This gives the motor more torque, which it needs to overcome the magnetic forces. Tools and extruders are wired the same way as in a standard multi extruder setup. Insert the magnets into the tools and lockplate, making sure to align the north/south poles so that the tool is pulled towards the lockplate when its "nose" is pointing straight up. Attach the tool head to the extruder carriage.

Attach a tool dock to the frame of your printer and make sure it is at the same height as the tools on the head. The holes at the bottom of the tool are supposed to slide over the pins of the dock when parking.

![image](https://user-images.githubusercontent.com/112611063/208246075-db46f666-d9a7-4499-bb7f-7939d0e811ee.png)

Modify your firmware with the toolchange code from this repository (currently only available for Klipper). Make sure you adjust the positions to suit your setup! Positions are best adjusted by moving the printer using G1 commands (G1 X10 Y50, for example) or whatever manual move command your printer may have. Make sure the head can clear any parked tools when moving to the next tool.

Slicer settings are the same as for normal multi extrusion printers, Klipper intercepts the T0/T1/... commands and adapts them to what the toolchanger needs.

# Versions

Currently, there are files for two versions on this repository. The prototype used pogo pins to electrically connect the tool, instead of wiring each tool individually. This has been more problematic than expected, so I discarded this for the "offical" V1 release. Mechanically, they are the same. More information is available in the prototype's readme.

# FAQs

Can I use this on my printer? What kind of tools can I use?

If your printer has a motion system that moves the head in both X and Y axes - such as a CoreXY - you only need a free stepper driver. Code is only available for Klipper right now, but it could probably be implemented in other firmwares too.
I recommend using only lightweight tools like Bowden hotends. The magnets aren't strong enough to reliably hold heavier tools. Tools like lasers or pen plotters are also possible, whatever you can design light enough can go on!

Why not use x instead?

A servo: For improved compatibility. Servos require a high current 5V power supply that's not available on all printers. Controlling it this way only needs a free motor driver.

Electromagnets: while they would work, they need to be constantly powered to keep holding the tool. That means the tool would fall off in the event of a power loss.

Permanent electromagnets: they're much heavier for the same force.

A physical lock: This would require precise parts that can't be produced on home 3D printers.

What happens if the nozzle hits a curled up edge?

From my testing so far, if it hits hard enough to overpower the magnets, it just flops up a little and then goes back on just fine. This actually makes it more resistant to print failures from what I've seen! If the tool falls off, there's a microswitch on the head that allows the controller to detect it (not yet implemented in software!).
