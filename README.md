KettleKontroller 1.0

by Cathal Garvey

This README is released under a Creative Commons Attribution, Sharealike License.

Introduction
============

Clever lab techniques can minimise the need for a waterbath, but sometimes you just need to get a sample to a precise temperature and keep it there for a long time. This should help.

The code is released under the GPL, and this README should be shared along with the code.

Usage
============

This code is designed for use on an Arduino Duemilanove, but will probably require zero modification to function on any given Arduino model.

To use this code, change "TargetTemp" to whatever temperature you want to keep the water at. Plug an LM35 into the arduino; V+ to 5V, Ground to Ground, and Vout to an analog pin (default A0). Connect the controlling pin (default 13) to one control pin of your relay, and the other pin to ground (some relays will only accept control voltage in one direction, meaning wires must be connected in a particular orientation). The relay should be (VERY CAREFULLY) spliced into the (UNPLUGGED) kettle power cable so that the live wire is controlled by the Relay. Only try this if you know what you are doing, and make sure your relay is capable of handling the wall voltage in your area and the current drawn by the kettle in use. Travel Kettles draw less current; use one of these. Seal the LM35 carefully so that it cannot be shorted by the water in the kettle, but not so thickly that it cannot read temperature. I used Sugru. Self-bonding silicone tape may work well for this task, also. Insulating the wires and crimping the entire sensor in tin might work well, too. Once you have a complete setup; Arduino, Kettle, Relay, Temperature Sensor, you can try to flash the Arduino and test it. The kettle should be switched on so that the Relay can control it.

Troubleshooting
============

1) Q: My Kettle doesn't turn on at all anymore, even if I connect the Relay Control pins directly to a suitable voltage supply in either orientation.

   A: You may have soldered/crimped the wires badly. Alternatively, your kettle might have drawn more current than the relay could handle, destroying it. Alternatively, your setup short-circuited the mains and blew the fuse on the kettle, possibly also the house (are the lights working?)

2) Q: My Kettle turns on, but just boils normally!

   A: Does it boil continuously, or does the power pulse on-and-off? That implies a fault in the control program. Does it just turn on and boil? Sounds like the relay isn't working, and the kettle's just working normally; are you using an AC relay with a DC power supply (apparently this leads to relay locking into always-on configuration). Have you modified the code to change the target temperature, and is that temperature close to 100C?

3) Q: My temperature control is stable, but at entirely the wrong temperature!

   A: Are you using an LM35, the sort that measures Celsius? If you are using a different chip you'll need to rewrite the ReadLM35 function.

4) Q: My temperatures are stable, but over/undershoot.

   A: Make sure the ReadLM35 function does at least 100 reads; the noise on these sensors can be significant, and incorrect reads mean incorrect thermostability. Try using the debug functions by changing the value of DebugMode to true.

5) Q: My temperatures vary a lot around the target temperature, or vary so much that they trip the failsafes.

   A: DON'T make the failsafes more permissive. Instead, play with the HeatPulseDur and RestPulseDur variables, which decide how temperature cycling occurs during a heating phase. Different kettles may require different settings; for a stable water bath, you want slow, gradual heating and stable temperature maintainance, so err on the side of a smaller HeatPulseDur vs. longer RestPulseDur; just enough to reach temperature.

Safety
============

The code defines several functions and calls them through the main loop() function. These functions include a failsafe function that tries to shut down the Arduino and drop the control pin to LOW if any unusual variations in temperature occur. This hopefully means that possible failure modes such as sensor-unplugging and water evaporation are covered.

For all that failsafes are included in the code, use of this code to control your own inventions is entirely a risk you undertake yourself, and I accept no responsibility for the safety of the code or any devices based on it.

TODO
============

1) temperature presets

2) dynamic cycling or timing

3) tidier code and fewer variables
