# Open-Source Magnetic 3D Printer ToolChanger Project

Magnetic tool changer project, inspired by E3D and Jubilee tool changing printers and designed for low cost and wide compatibility. My goal is to make toolchanging accessible without owning four figure machines.

# How does it work?

Both the tools and the toolhead have a circle of neodymium magnets with alternating north/south up polarities. Opposing poles attract and same poles repel, it is therefor possible to switch between attraction and repulsion by aligning the opposing or same poles of the magnets. This is done by a geared stepper motor on the toolhead which rotates the magnet circle. Steel balls and L profiles form a Maxwell coupling which ensures repeatable connections by having the tool contact the head in exactly 6 points - thus eliminating all 6 degrees of freedom, but not overconstraining the assembly.
The Klipper firmware is capable of controlling such a setup using the manual stepper feature and custom gcode macros.

I'm working on documenting a more detailed explanation.

# How do I use it?

Klipper code, CAD files and assembly drawings are documented on here. You'll need to design a carriage for your printer (please share it with the community if you do!). I'm working on a more detailed guide. Be aware that this is very much a prototype - you're welcome to test it and make any changes you want, but please don't leave the printer unattended. I'm not responsible for any failures that may occur.

The 28BYJ-48 stepper motor can be driven with a normal stepstick with some minor modifications, you only need some basic tools. Follow the guide at https://www.youtube.com/watch?v=kq04-ReW1xY (it's also possible to use a knife instead of a Dremel!). Detailed electrical documentation is a work in progress, wire the head pins to a heater, each standby dock to another heater and the head motor to a free driver. Don't run the motor above 400mA or you may break the gearbox!

# Version 1 information

The first prototype uses pogo pins to electrically connect the tool. These have proven to be more trouble than they're worth and I will replace them with a manual quick disconnect or plug on the machine frame in version 2. If you want to build yourself a toolchanger right now, I would recommend wiring the tools like normal bowden hotends and skipping the pins.
Using the pins currently means you lose some thermistor protection features since you have to fool the controller into thinking there's still a thermistor on the head when there is no tool. This is accomplished by the microswitch connecting a resistor in parallel to the thermistor when the tool is disconnected. Make sure you enable the VERIFY_HEATER feature in Klipper, or the equivalent in your firmware to protect against improperly connected tools! This also means you are currently limited to using the same thermistor type on all tools. This will change with V2.

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

From my testing so far, if it hits hard enough to overpower the magnets, it just flops up a little and then goes back on just fine. If the tool falls off, there's a microswitch on the head that allows the controller to detect it.
