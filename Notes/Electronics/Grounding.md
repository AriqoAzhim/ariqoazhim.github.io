# Ground

**Misconceptions**

- “Gnd” has zero voltage, zero resistance, zero impedance etc. NOT TRUE, this is because as there is a current running through a conductor a voltage drop occurs. This is only near true in a DC environment but only in an ideal environment (0 ohm resistance conductor, no inductance etc)
- Ground is thought of as a sinkhole for noise. NOP

**What is energy?**

a property of matter that manifests the ability to perform work. Light and Electrical Energy are actually the same with the only difference being frequency where light is a million times higher in freq than a 500Mhz signal.

**Is energy in the voltage or current?**

Neither, it’s in the magnetic and electric fields of the circuit

*BUT where are they?*

- traces?
- planes?

NEITHER again lol

It actually travels in the dielectric of the board. the FR4 Plastic/Fibreglass material. In the space between the traces and the plane. No energy travels in the copper

*Nani??*

The energy is called an Electromagnetic wave and the copper trace acts as a guide for the waves since it is the path of least impedance. You could send signals into the dielectric if you wanted. We do it with Radio, TV, Wifi. Bluetooth etc. The purpose of the trace is to guide the energy so that if we release a wave into a dielectric we are guiding it where we want it

A transmittion line (wave guide) is a pair of conductors that facilitate the movement of energy from point A to point B

- Voltage travels across the copper of the transmission line while current travels in the copper
- The energy is in the FIELDSSS in the dielectric

There is a misconception that the energy travels from the source to the load, then returns to gnd. However, this is WRONG. Instead, the energy is launched into the circuit in waves consisting of fields. These fields are created in the both conductors at the same time and create a forward and return current **SIMULTANEOUSLY**.  The consideration of the GND is tantamount to the performance of the circuit

This illustrates an example of a trace contained within two planes (could be gnd plane pwr plane etc)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/85a4163e-baed-4010-a7d0-bfe321934e6d/Untitled.png)

- inner layer trace fields are constrained

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3943608a-228d-49d4-a0bf-f8c9e4d2a93f/Untitled.png)

- Cross talk is worse on outer layers due to these waves begin less constrained

At 0Hz current would take the path of least resistance in a circuit. So the return current from the following signal to GND would travel as follows (also spreading out, making the return signal as wide as it is long, effectively making a square, resistance is a per square measurement). This would only apply if the signal line was just  a non changing value, i.e 5V from a PSU. Things change when we start talking about state changing signals…

Why?

At low frequencies current will take the path of least resistance, since impedance of ‘L’ becomes negligiable compared to ‘R’ in the following. Where jw  is a frequency factor. At low frequencies, the R is more prononced. Especially at DC, where frequency = 0 and thus jw goes to 0.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99134069-4993-44ec-a0c2-5a14958a7e5c/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be91e7a3-301d-4d89-957e-8be50f5435ca/Untitled.png)

If the frequency of the signal gets higher, the more it tends to return through the GND plane directly under the signal trace. Definitely if it’s above the audio frequencies (20khz and above). 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f4154a4-4711-4b87-a943-b8cfcc000b3e/Untitled.png)

But Why? this is because the signal will **ALWAYS** take the path of least **IMPEDANCE.**  which is the

$$
Z_o = \sqrt(L_o/C_o)
$$

where L = Inductance, and C = Capacitance. So it would want a path with the **Lowest Inductance** and **Highest capacitance**

**Inductance** = An impedance to change in current flow, caused by the mass (inertia) of the magnetic field. i.e 

That’s why when you have decoupling caps that have vias to there respective planes/traces it’s best to keep the vias as close as possible . This minimized spread of both fields and creates low impedance paths with low inductive losses

**Consider the following:**

the shortdashed outline is a trace on Layer 1 and the Gray portion is the ground plane on layer 2

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a45b715-5665-4537-86e7-1b7a6b65c9a3/Untitled.png)

Intuitively we can say that 1 and 3 should be similar since the ground plane under the trace is uncut and hence the return signal does not need to propagate much to return to GND. But what about 2 and 4? Which would be worse?

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/28d9dd2f-e28f-4764-a42d-12743cc7cbff/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae13f3f2-1537-4218-9235-bda23dacd1e9/Untitled.png)

Turns out 2 is the worst. Remember that the return signal energy is travelling through the dielectric, coupled to the forward signal trace and when it meets the slot, it spread out along the slot to find the return path, causing a 20-30dB worse EMI signature than any of the others. 

**RULE NUMBER 1 :: NEVER ROUTE OVER SPLITS IN THE GROUND PLANE**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ddd7fccf-1700-4d81-b997-f45c4fabd61b/Untitled.png)

This is a 2 layer board without any planes, just poured copper. This is gnna be dogshit. So what do you do?

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6751d4c9-035a-423c-b26a-674f293ac13e/Untitled.png)

add ground lines in between pairs. Again, remember that energy travels through the path of least impedance, and the addition of the ground traces helps to guide the return paths of the signals instead of letting them spread till they find the GND reference. BTW, this is still not good, but in the case where you need to get things done in 2 layers, this is the best solution. In the above example, these changes resulted in a 10-20dB improvement in the EMI signature of the board. There HAS to be an intentional low impedance ground nearby or you’re going to get a noisy disaster of a board.

Also notice that there is a ground pour under the microcontroller IC. Why? This is to reduce the field size of the Microcontroller’s energy field by coupling the field to that space creating a low impedance attachment point. Kind of like shielding.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b7d5256-57dd-4c41-9a7a-a64d3b72ec3b/Untitled.png)

It is okay to route power to boards. A power plane is typically unneeded in boards using lower frequencies. ( should be g for anything I design).

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/59f1e97a-bfbc-4f64-be97-6a0c79e97cf0/Untitled.png)

4 layer boards typically come like the above, where there is a distinctly larger delta between layers 2 and 3 than any other layer. 

Here’s a comparison between 2 and 4 layers

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f1dea6f-7feb-424a-844f-4cabe02d535d/Untitled.png)

Back to the problem. In terms of Signal Integrity, this will not cause any issues even if you go into the Ghz of frequency signals. However, it’s another story for EMI

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a4d77ca-7c09-4162-9111-5cbf79f35d15/Untitled.png)

How do we fix it? Same as before route signals in layer 1 in triplets like before

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa0f1979-f4cc-4f93-98d7-e9eade6e47e0/Untitled.png)

Where signal pairs have a common GND trace routed next to them.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e894b31-83de-4718-a47a-243b98cbe647/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f2072a8f-e9da-46f1-a705-8b5a656120c6/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b021c9be-628f-43a9-aa69-f048361241f7/Untitled.png)

Group components by function/Family

- Digital one one side and analog on another
    - Low Level Analog in one area
    - High Freq in another
    - RF/Microwave in it’s own area
    - If traces are isolated to their own sections, the need to split analog and digital ground into their own sections is only necessary when the analog is operating under 20KHz
- Devices with different voltages in different areas
    - This will mitigate crosstalk problems due to the different voltages from different signals crossing each other.
    - You’ll be able to reduce the amount of power planes in a board if you set up the board correctly
- Devices operating at different frequencies
- By function within a given family or voltage
- All ICs should be routed to connectors that are very close.
    - If there are long routes from that IC to the connector, the common mode currents and energy will couple into the traces and go to the next board or go out into a couple and drive the cable as an antennae and you’re going to have an EMI failure.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97c02274-e9fb-402c-b7fd-0826e77b5b89/Untitled.png)

20H refers to 20 times the distance of the dielectric spacing between the top layer and the ground copper layer under it. Referring to the below diagram, that would be 20*6.7mil = 134mil = 3.4036 mm

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f1dea6f-7feb-424a-844f-4cabe02d535d/Untitled.png)

This will ensure that the fields from the analog and digital signals do not couple with each other or ar minimised to an amount that will not cause EMI issues.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cdcdf149-73b0-4cc5-b29b-d7a00daea1a5/Untitled.png)

Instead, you only need to split one of the planes, power or gnd and since we want a continuos ground for our signals to couple to, you split the power plane only instead of splitting the Ground. This idea of splitting the ground plane and Power plane comes from the idea that the energy travels through the copper. THIS IS WRONGGGGG. the energy travels through the dielectric space between the planes.  

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e01ba337-8e98-4199-9220-38c0e914e1ed/Untitled.png)

## Study by Dr Bruce Archambeault

This was to verify the conclusion that you SHOULDN’T route over split power planes given a longer dielectric layer ( more than 8 mil). There were 4 cases for a driver and receiver circuit tested for EMI

1. One where the Circuit and Driver were routed on the top layer copper with GND being referenced in the 2nd copper layer

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5ebf571-89c9-4892-b7c5-69128b3d8dd0/Untitled.png)

1. One where the Circuit and Driver were on the Bottom layer (mirrored) with the a split power plane on layer 3 being referenced. This would have left a dielectric space between the bottom layer and GND layer at >35mil. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/67b30fe1-3300-4517-be87-71c5ae897743/Untitled.png)

1. Then 0.01 uF caps were added between the power planes ( through Vias I guess) to mitigate the issue of 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/371e936b-cd90-405c-afe7-ded2fcb35899/Untitled.png)

1. Then added another set

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/172c7442-d067-4c3f-bb88-08b34150f34d/Untitled.png)

Here are the results:

Blue: 1

Orange: 2

Purple: 3

Green: 4

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e332d77-4be3-4221-9d55-ae18ae3c5d5d/Untitled.png)

Why did the caps only help during low frequencies?

A capacitor has an impedance due to capacitance that start high and gets lower as frequency increases

It also has an impedance due to inductance (due to paltes in the capacitor, pins of metal in the IC). That inductance starts low and increases with frequency.

At a certain freq namely the **Self Resonant Frequency** that value will be the same where they will be 180 degrees out of phase. Meaning that they will cancel each other out. This is the ideal scenario for a capacitor as the only impedance would come from the Resistance in the structure (refer to impedance formula)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/835df32b-e96f-471c-926a-4606717bca37/Untitled.png)

Hence the overall frequency response is illustrated by the orange curve in the above graph.

Before the self resonant frequency, the capacitor would have a higher impedance but it would be due to capacitive impedance. BUT on the right hand side, it would have higher impedance due to inductance. The capacitor becomes  a bigger and bigger inductor and there comes a point where it doesn't serve it’s purpose anymore as seen in the Bruce Archambeault experiment.

Do not route signals on GND planes unless you really need to. This will cause Field Expansion and Coupling between the lines.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b85f27b-8e05-4d1c-b09b-7872fd987b7b/Untitled.png)

 However, if you really need to, make sure of the following 

- Keep Ground plane opening short
- Opening should be shorter in inches than the signal rise time in Nsec, to help contain fields and control Common Mode current and interference
- Are they sensetive analog signals? DONT DO ITTTT. find another way.

- Try using a 0hm resistor to jump over the signals. Remember that if you try this with high frequency signals it will most likely not work since, the 0ohm resistor will not be 0hm at those frequencies.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9e40b36-235e-43fc-bbc8-0cdf05d80cde/Untitled.png)

These are not ideal but will work for low frequency applications when there are no other options like in situations where you are making low count layer boards.  \

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fbcc8365-6403-4fbe-b688-63bb72530f57/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e2835ee-1e59-4bc5-8dd4-d43bbaefabd1/Untitled.png)

Not required for Signal Integrity unless it was a senseitve analogue circuit. Would put 4GND vias around the signal to contain the field.

If you’re space constrained, try to group up signal vias around a singular ground via. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd5fc565-b781-4ef1-864a-237c5536483d/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3e0b4b2f-ec31-4a1f-9c69-85d7c8567108/Untitled.png)

What if the distance between layers is high (>35 mil)? Such as in layer 2 and 3 of a 4 layers stack and 3 and 4 of a 6 layers stack

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee4d9258-d02f-438e-9c10-8ff2edcb5da8/Untitled.png)

# Layer Stacks

Recommended Stacks

## 2 Layers

Layer 1: Signal + Power

Layer 2: Ground

## 4 Layers

**Layer 1:** Ground

**Layer 2:** Signal + Power

**Layer 3:** Signal + Power

**Layer 4:** Ground

OR

**Layer 1:** Signal + Power 

**Layer 2:** Ground

**Layer 3:** Ground

**Layer 4:** Signal + Power

## 6 Layers