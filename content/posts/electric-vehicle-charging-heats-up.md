---
title: "Electric Vehicle Charging Heats Up"
date: 2025-01-23
---

![](https://hackaday.com/wp-content/uploads/2025/01/charger-heater-main.png?w=800)

As the electric vehicle takeover slowly lumbers along, marginally increasing efficiencies for certain applications while entrenching car-centric urban design even further, there are some knock-on effects that are benefiting people and infrastructure beyond simple transportation. Vehicle-to-grid technology has applications for providing energy from the car back to the grid for things like power outages or grid leveling. But \[Technology Connections\] is taking this logic one step further. Since a large number of EV owners have charging stations built into their garages, he wondered if these charging stations could be used for other tasks and built an electric heater which can use one for power.

This project uses a level 2 charger, capable of delivering many kilowatts of power to an EV over fairly standard 240V home wiring with a smart controller in between that and the car. Compared to a level 1 charger which can only trickle charge a car on a standard 120V outlet (in the US) or a DC fast charger which can provide a truly tremendous amount of energy in a very short time, these are a happy middle ground. So, while it’s true a homeowner could simply wire up another 240V outlet for this type of space heater or other similar application, this project uses the existing infrastructure of the home to avoid redundancies like that.

Of course this isn’t exactly plug-and-play. Car chargers communicate with vehicles to negotiate power capabilities with each other, so any appliance wanting to use one as a bulk electric supply needs to be able to perform this negotiation. To get the full power available in this case all that’s needed is a resistor connected to one of the signal wires, but this won’t work for all cases and could overload smaller charging stations. For that a more complex signalling method is needed, but since this was more of a proof-of-concept we’ll still call it a success. For those wanting to DIY the charger itself, building one from the ground up is fairly straightforward as well.

Thanks to \[Billy\] for the tip!

<iframe loading="lazy" title="I tricked my car charging station into powering a 7.5 kW heater" width="800" height="450" src="https://www.youtube.com/embed/kTctVqjhDEw?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
