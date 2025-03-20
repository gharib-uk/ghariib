---
title: "Building an electric vehicle simulator to research EVSEs"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

Researching and reverse engineering Level 2 Electric Vehicle Supply Equipment (EVSE or loosely “charger”) efforts might require the equipment to be placed beyond the idle state. The idle state is straightforward and usually involves nothing more than powering up the charger. Indeed, this is a very useful state for research where the user interface is in operation, communications both wired and wireless are working and the mobile device app can interact. However, there are times when there is a need to force the charger into other states so that it behaves as though the electric vehicle is attached, the EV is asking for charge, or the EV is charging and the EVSE is providing charging current.  

At the Pwn2Own Automotive 2025 event, an add-on category was introduced that required a demonstration of an exploit while the equipment was in the charging state. This required manipulation of the charger via the charging cable in order to enter this charging state. This blog describes the device that we assembled to achieve this requirement. I will cover the design considerations that were made along with how the device operates. I will also provide the specifics on parts that we used; however, due to the wide range of requirements different researchers might have and substitutions they might make, I will only highlight the important points of the design and not describe this as a “step-by-step” build.  

As an example of a minimal “simulator” build, some chargers have been observed to provide an output signal when the charging cable attaches to the vehicle. If your research only involves this signal, then a diode and a single resistor inside your EV simulator enclosure may be enough to achieve your goal. So, understand our device first and then use that knowledge to build what you require for your research.

**Safety**

First and foremost is safety. If you are familiar with researching EVSE, you know that you will be dealing with deadly voltages. This is the case here as well. Even more dangerous is the fact that the EV simulator will enable the high voltage onto the charging cable and inside the simulator itself. This increases the number of components that have high voltage and thus increases the danger. Every precaution and safety measure should be taken in dealing with high voltage and high current. If you are not knowledgeable and confident with working around deadly high voltages or are not sure of appropriate safety measures, do not attempt to build a simulator or work with EVSE. An alternative might be to educate yourself and ask others with the appropriate qualifications for help.  As we were about to publish this blog, we found some pre-built devices that are limited but might still be an option for some.  These are listed in the “Pre-Built Alternatives” section below.

**Basic Operation**

The EV simulator is based on SAE J1772 operation. There are other standards that define EVSE to EV connections but our focus in this blog is strictly J1772.

The basic information needed can be found here.

The key items this implies are:

J1772-2009 connector is used.  
Control signaling (over the Control Pilot signal) is strictly PWM/duty cycle.

If you study the wiki link above, you will see that the EV communicates to the EVSE on the CP line via a resistor network in the vehicle. The EVSE will sense a resistance to determine if the cable is connected to the EV (2740 ohms) and if the vehicle wants charging (882 ohms). On the flip side, the EVSE provides a 1kHz PWM signal to the vehicle to indicate the maximum current that the EVSE is willing to provide to the EV. This is done through the duty cycle of the signal. The duty cycle to current mapping is defined by J1772.

The EV simulator provides the proper resistance values using a rotary switch and monitors the PWM signal with a low-cost onboard oscilloscope. These parts reside on or inside a plastic enclosure to provide safety from high voltage and anchor the main components.

In our EV simulator, the rotary switch is set to “0” when we want to simulate a disconnected cable. It is set to “1” to simulate a connection to a vehicle but no charge being requested. Finally, it is set to “2” to simulate a connected vehicle requesting a charge. These are the only three states that we decided to incorporate into our simulator.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/f016aea6-41f8-4c39-a91a-1437293cbb8c/Picture1.jpg?format=1000w)

<figcaption>

_Figure 1 – The EV Simulator at the time of P20 Automotive Tokyo 2025_

</figcaption>

</figure>

**Parts**

This is a list of components that we used to build our EV simulator and their links to Amazon USA. The resistors come in a kit, and we used 2700ohm for the 2740ohm requirement and 820ohm for the 882ohm requirement. These approximate values appeared to work on the EVSEs that we tested. However, you could find a more particular EVSE that needs values closer to the specification. In that case, you could combine other resistors in the kit to get closer to the specification.

As for the diode, you should try to use the one we used or one with even lower Vf specification. This diode worked with all of the chargers we were able to test. We tried higher Vf diodes with limited success. Some chargers were more tolerant of a higher Vf but many were not. The diode is required because most EVSE will test for its existence as a safety measure. 

·      Project box: LeMotech Junction Box  
·      Receptacle: J1772 receptacle  
·      Resistor 2740 ohm and 882 ohm: Chanzon 300 Piece Resistor Kit  
·      Diodes: Chanzon 1N5817 Schottky Barrier Rectifier Diodes  
·      Rotary switch: Taiss Universal Changeover Switch  
·      Volt/Amp Meter: DROK Volt Amo Meter  
·      Oscilloscope: FNIRSI DSO152 Oscilloscope  
·      Load Plug: 20FT NEMA 6-15P/6-15R Power Extension Cord  
·      Load: Cadet F Series Baseboard Heater

**Considerations**

A few of the items are not strictly necessary but do help in verifying the simulator is working as expected. Substitutions are also possible depending on your needs.

The bare minimum is the enclosure, the J1772 receptacle, and the proper resistor and diode values. How you manipulate the resistor network, monitor the PWM signal, and if you do or do not implement a load is flexible and up to you.

**Assembly of (Our) EV Simulator**

Our initial step in assembly was to drill or cut holes into the plastic enclosure. Placement isn’t critical; however, we did attempt to make things ergonomic and to not block the indicators with cables or our hands while manipulating the switch. Secondly, we connected the resistors and diode to the rotary switch and mounted that assembly inside the enclosure. This circuit connects to the Control Pilot (CP) and Protective Earth (PE) inside the enclosure.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/97673eb5-fa37-484e-96b8-fee6006a45e7/Picture2.png?format=1000w)

<figcaption>

_Figure 2 – Schematic of components connected to the rotary switch._

</figcaption>

</figure>

Again, this was our implementation. We left S0 on our switch open to simulate a cable in the unconnected state without having to physically plug and unplug the cable. We also chose to control the two resistor states (S1 and S2) separately. The schematic in the wiki shows a different configuration in which only one toggle is required, and the values of the resistors are adjusted.  Both are valid so build based on your constraints. Finally, as seen in the schematic, we did not utilize states 3 -5 on the rotary switch because the additional charging states were not needed for our research at the time.

Next, the J1772 receptacle needs to be wired.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c80d05d0-1469-44b5-8e32-325b40ef299b/Picture3.png?format=1000w)

<figcaption>

_Figure 3 – The J1772 charge connector as seen from the open end_

</figcaption>

</figure>

If you choose not to add a load to the simulator, the wiring is fairly easy. You only need to bring out the Control Pilot (CP) signal and the Protective Earth (PE) from the back of the connector. If you want to include a simulated battery load, you will need to also bring out the two high voltage lines (L1 and L2/N) with the appropriate size wire (remember, these can be high current depending on the load you pick). We conservatively used a 12-gauge wire, and our anticipated load was 500W. L1 and L2/N were then routed out of the enclosure to a plug so that the 500W load could be removed or added at will. The plug (in the material list above) was a heavy-duty extension cord that we cut and hardwired to the enclosure on one side and to the load on the other side. We also included a Volt/Amp meter to monitor the J1772 receptacle. When the EVSE engaged charging, the meter would show 230V (in our case) and 2.2A if the load was attached otherwise, 0A. Finally, an inexpensive battery-powered oscilloscope was attached with double-sided tape to the front of the enclosure. The probe cable was routed inside to monitor the PWM signal present on the CP wire. This simple scope defaulted to displaying several measurements, which included a duty cycle, so it was a good fit for this purpose. Other than pressing “Auto Set” once the 1KHz signal was present, there was no other configuration required.

Thus, the full enclosure schematic is:

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c0f98406-75f7-4618-80d3-5209ebcfcae6/Picture4.png?format=1000w)

<figcaption>

_Figure 4 – The full schematic of our EV simulator_

</figcaption>

</figure>

**Targets**

This EV simulator was evaluated on all of the targets used at Pwn2Own Automotive 2025. This included Level 2 chargers from Autel, ChargePoint, WOLFBOX, Emporia, Ubiquity, and Tesla (with NACS adaptor). All the EVSE devices were successfully placed into the charging state and 500W was provided to the load using this simulator.

**Pre-Built Alternatives** 

As mentioned in the “Safety” section, we noticed a few consumer devices, which might be adequate for some research and would avoid having to assemble any parts. These devices appear to be designed primarily to charge electric scooters from an EVSE. To do this, they effectively provide the minimum circuit to enable the EVSE to energize the cable. Note that these do not provide a direct way to measure the CP signal.  Always follow the manufacturer’s safety instructions when using these devices.

·      220V J1772 Type1 Socket to NEMA 5-15/5-20 EV Charger Adapter  
·      J1772 to Nema 5-20, EV Station Charging Adapter

**Final Thoughts** 

The J1772 standard is straightforward to implement for the limited purpose of emulating an attached vehicle to these chargers. There are more sophisticated protocols on the horizon (CAN over the CP signal) but most of the consumer-grade EVSEs continue to utilize J1772. Additionally, with such a large established base of J1772, support from the EVSEs and the EVs will likely continue far into the future.  Hopefully, this blog describing our design considerations and how we built our EV simulator will simplify the process for other researchers. While official contest specifics are still months away for the Pwn2Own Automotive event for 2026, there will almost certainly be a contest category for using the charging cable as the attack surface again. This was a highlighted addition to the 2025 event over 2024 and we hope to build on that for 2026.

Until then, you can follow us on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Researching and reverse engineering Level 2 Electric Vehicle Supply Equipment (EVSE or loosely “charger”) efforts might require the equipment to be placed beyond the idle state. The idle state is straightforward and usually involves nothing more than powering up the charger. Indeed, this is a very useful state for research where the user interface is in operation, communications both wired and wireless are working and the mobile device app can interact. However, there are times when there is a need to force the charger into other states so that it behaves as though the electric vehicle is attached, the EV is asking for charge, or the EV is charging and the EVSE is providing charging current.  

At the Pwn2Own Automotive 2025 event, an add-on category was introduced that required a demonstration of an exploit while the equipment was in the charging state. This required manipulation of the charger via the charging cable in order to enter this charging state. This blog describes the device that we assembled to achieve this requirement. I will cover the design considerations that were made along with how the device operates. I will also provide the specifics on parts that we used; however, due to the wide range of requirements different researchers might have and substitutions they might make, I will only highlight the important points of the design and not describe this as a “step-by-step” build.  

As an example of a minimal “simulator” build, some chargers have been observed to provide an output signal when the charging cable attaches to the vehicle. If your research only involves this signal, then a diode and a single resistor inside your EV simulator enclosure may be enough to achieve your goal. So, understand our device first and then use that knowledge to build what you require for your research.

**Safety**

First and foremost is safety. If you are familiar with researching EVSE, you know that you will be dealing with deadly voltages. This is the case here as well. Even more dangerous is the fact that the EV simulator will enable the high voltage onto the charging cable and inside the simulator itself. This increases the number of components that have high voltage and thus increases the danger. Every precaution and safety measure should be taken in dealing with high voltage and high current. If you are not knowledgeable and confident with working around deadly high voltages or are not sure of appropriate safety measures, do not attempt to build a simulator or work with EVSE. An alternative might be to educate yourself and ask others with the appropriate qualifications for help.  As we were about to publish this blog, we found some pre-built devices that are limited but might still be an option for some.  These are listed in the “Pre-Built Alternatives” section below.

**Basic Operation**

The EV simulator is based on SAE J1772 operation. There are other standards that define EVSE to EV connections but our focus in this blog is strictly J1772.

The basic information needed can be found here.

The key items this implies are:

J1772-2009 connector is used.  
Control signaling (over the Control Pilot signal) is strictly PWM/duty cycle.

If you study the wiki link above, you will see that the EV communicates to the EVSE on the CP line via a resistor network in the vehicle. The EVSE will sense a resistance to determine if the cable is connected to the EV (2740 ohms) and if the vehicle wants charging (882 ohms). On the flip side, the EVSE provides a 1kHz PWM signal to the vehicle to indicate the maximum current that the EVSE is willing to provide to the EV. This is done through the duty cycle of the signal. The duty cycle to current mapping is defined by J1772.

The EV simulator provides the proper resistance values using a rotary switch and monitors the PWM signal with a low-cost onboard oscilloscope. These parts reside on or inside a plastic enclosure to provide safety from high voltage and anchor the main components.

In our EV simulator, the rotary switch is set to “0” when we want to simulate a disconnected cable. It is set to “1” to simulate a connection to a vehicle but no charge being requested. Finally, it is set to “2” to simulate a connected vehicle requesting a charge. These are the only three states that we decided to incorporate into our simulator.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/f016aea6-41f8-4c39-a91a-1437293cbb8c/Picture1.jpg?format=1000w)

<figcaption>

_Figure 1 – The EV Simulator at the time of P20 Automotive Tokyo 2025_

</figcaption>

</figure>

**Parts**

This is a list of components that we used to build our EV simulator and their links to Amazon USA. The resistors come in a kit, and we used 2700ohm for the 2740ohm requirement and 820ohm for the 882ohm requirement. These approximate values appeared to work on the EVSEs that we tested. However, you could find a more particular EVSE that needs values closer to the specification. In that case, you could combine other resistors in the kit to get closer to the specification.

As for the diode, you should try to use the one we used or one with even lower Vf specification. This diode worked with all of the chargers we were able to test. We tried higher Vf diodes with limited success. Some chargers were more tolerant of a higher Vf but many were not. The diode is required because most EVSE will test for its existence as a safety measure. 

·      Project box: LeMotech Junction Box  
·      Receptacle: J1772 receptacle  
·      Resistor 2740 ohm and 882 ohm: Chanzon 300 Piece Resistor Kit  
·      Diodes: Chanzon 1N5817 Schottky Barrier Rectifier Diodes  
·      Rotary switch: Taiss Universal Changeover Switch  
·      Volt/Amp Meter: DROK Volt Amo Meter  
·      Oscilloscope: FNIRSI DSO152 Oscilloscope  
·      Load Plug: 20FT NEMA 6-15P/6-15R Power Extension Cord  
·      Load: Cadet F Series Baseboard Heater

**Considerations**

A few of the items are not strictly necessary but do help in verifying the simulator is working as expected. Substitutions are also possible depending on your needs.

The bare minimum is the enclosure, the J1772 receptacle, and the proper resistor and diode values. How you manipulate the resistor network, monitor the PWM signal, and if you do or do not implement a load is flexible and up to you.

**Assembly of (Our) EV Simulator**

Our initial step in assembly was to drill or cut holes into the plastic enclosure. Placement isn’t critical; however, we did attempt to make things ergonomic and to not block the indicators with cables or our hands while manipulating the switch. Secondly, we connected the resistors and diode to the rotary switch and mounted that assembly inside the enclosure. This circuit connects to the Control Pilot (CP) and Protective Earth (PE) inside the enclosure.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/97673eb5-fa37-484e-96b8-fee6006a45e7/Picture2.png?format=1000w)

<figcaption>

_Figure 2 – Schematic of components connected to the rotary switch._

</figcaption>

</figure>

Again, this was our implementation. We left S0 on our switch open to simulate a cable in the unconnected state without having to physically plug and unplug the cable. We also chose to control the two resistor states (S1 and S2) separately. The schematic in the wiki shows a different configuration in which only one toggle is required, and the values of the resistors are adjusted.  Both are valid so build based on your constraints. Finally, as seen in the schematic, we did not utilize states 3 -5 on the rotary switch because the additional charging states were not needed for our research at the time.

Next, the J1772 receptacle needs to be wired.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c80d05d0-1469-44b5-8e32-325b40ef299b/Picture3.png?format=1000w)

<figcaption>

_Figure 3 – The J1772 charge connector as seen from the open end_

</figcaption>

</figure>

If you choose not to add a load to the simulator, the wiring is fairly easy. You only need to bring out the Control Pilot (CP) signal and the Protective Earth (PE) from the back of the connector. If you want to include a simulated battery load, you will need to also bring out the two high voltage lines (L1 and L2/N) with the appropriate size wire (remember, these can be high current depending on the load you pick). We conservatively used a 12-gauge wire, and our anticipated load was 500W. L1 and L2/N were then routed out of the enclosure to a plug so that the 500W load could be removed or added at will. The plug (in the material list above) was a heavy-duty extension cord that we cut and hardwired to the enclosure on one side and to the load on the other side. We also included a Volt/Amp meter to monitor the J1772 receptacle. When the EVSE engaged charging, the meter would show 230V (in our case) and 2.2A if the load was attached otherwise, 0A. Finally, an inexpensive battery-powered oscilloscope was attached with double-sided tape to the front of the enclosure. The probe cable was routed inside to monitor the PWM signal present on the CP wire. This simple scope defaulted to displaying several measurements, which included a duty cycle, so it was a good fit for this purpose. Other than pressing “Auto Set” once the 1KHz signal was present, there was no other configuration required.

Thus, the full enclosure schematic is:

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c0f98406-75f7-4618-80d3-5209ebcfcae6/Picture4.png?format=1000w)

<figcaption>

_Figure 4 – The full schematic of our EV simulator_

</figcaption>

</figure>

**Targets**

This EV simulator was evaluated on all of the targets used at Pwn2Own Automotive 2025. This included Level 2 chargers from Autel, ChargePoint, WOLFBOX, Emporia, Ubiquity, and Tesla (with NACS adaptor). All the EVSE devices were successfully placed into the charging state and 500W was provided to the load using this simulator.

**Pre-Built Alternatives** 

As mentioned in the “Safety” section, we noticed a few consumer devices, which might be adequate for some research and would avoid having to assemble any parts. These devices appear to be designed primarily to charge electric scooters from an EVSE. To do this, they effectively provide the minimum circuit to enable the EVSE to energize the cable. Note that these do not provide a direct way to measure the CP signal.  Always follow the manufacturer’s safety instructions when using these devices.

·      220V J1772 Type1 Socket to NEMA 5-15/5-20 EV Charger Adapter  
·      J1772 to Nema 5-20, EV Station Charging Adapter

**Final Thoughts** 

The J1772 standard is straightforward to implement for the limited purpose of emulating an attached vehicle to these chargers. There are more sophisticated protocols on the horizon (CAN over the CP signal) but most of the consumer-grade EVSEs continue to utilize J1772. Additionally, with such a large established base of J1772, support from the EVSEs and the EVs will likely continue far into the future.  Hopefully, this blog describing our design considerations and how we built our EV simulator will simplify the process for other researchers. While official contest specifics are still months away for the Pwn2Own Automotive event for 2026, there will almost certainly be a contest category for using the charging cable as the attack surface again. This was a highlighted addition to the 2025 event over 2024 and we hope to build on that for 2026.

Until then, you can follow us on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
