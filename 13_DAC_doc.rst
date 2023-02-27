Digital to Analog Converter
============================

Introduction
---------------
There are several options available for digital synthesis of higher frequency signals if PWM is limited in bandwidth. In this discussion, we will explore three options (or four, depending on whether or not derivatives are included). So, let's get started with the first DAC architecture, without any further delay.

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/Ym0sGiuoVR4" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Resistor voltage divider
-------------------------------
The easiest DAC to understand is a string of resistors that creates a resistor divider with multiple output nodes. By selecting which of these voltages are connected to the output, we can obtain a basic but effective DAC. The image below depicts a three-bit DAC.

.. image:: img/13_resistor_DAC_schematic.png
	:name: resistor voltage divider DAC schematic
	:align: center

Although it may seem simplistic, the DAC that utilizes a string of resistors is a legitimate method for digital-to-analog conversion. However, it does have a significant drawback in that it requires a large number of resistors with the same resistance. Even slight differences in resistance between the resistors can result in nonlinearity in the output. In fact, this method requires 2^N resistors, where N is the number of bits in the DAC.

To construct such a DAC, we can use a jumper wire instead of multiple switches and a decoder. However, in a real-world application, an analog multiplexer/demultiplexer such as 74HC4051 would be used to handle the switches and decoding.

.. image:: img/13_resistor_divider_circuit.jpg
	:name: resistor voltage divider DAC circuit
	:align: center

Another disadvantage of this circuit is that by adding more bits, number of switches increases. On its own, this is not a problem, but unfortunately, each switch adds parasitic capacitance, which limits maximum operation frequency. This problem can be mitigated by using a staged design. Here first DAC with half the bits generates reference voltage for the second stage DAC.

.. image:: img/13_staged_resistor_DAC_schematic.png
	:name: staged resistor voltage divider DAC schematic
	:align: center

Example on picture realizes a 6 bit DAC but only uses double the amount of components instead of four times more. In general, a two staged design requires :math:`2 \cdot 2^{N/2}` resistors and switches but yes, it requires two voltage buffers. Aside from reducing parasitic capacitance, this design also lets us double bit count of DAC. Adding even more stages is possible, but at some point noise will overshadow lowest bits.
If you want, you can test this circuit yourself, but we won’t be losing any more time with this variant. Time to find a DAC that requires fewer components.

Binary weighted resistor DAC
-----------------------------------
Remember the chapters about analog addition OpAmp amplifiers? If not let's quickly recall the circuit.

.. image:: img/7_OpAmp_adder.png
	:name: OpAmp adder
	:align: center

	.. math:: U_{OUT} = -(U_1 \cdot \frac{R_2}{R_1} + U_2 \cdot \frac{R_2}{R_3})
	
By adding more input voltages and input resistors, the equation for the output voltage would simply be extended. However, if the input voltages are fixed at either Uref or 0V, and the resistor values are fixed as well, the equation becomes simpler. In this example, b0 through b3 represent the bits that control the state of the resistors. A value of 1 for a bit means that the corresponding resistor is connected to the reference voltage Uref, while a value of 0 means that the corresponding resistor is connected to 0V.

.. image:: img/13_binary_weighted_DAC_schematics.png
	:name: binary weighted DAC schematic
	:align: center

	.. math:: U_{OUT} = - U_{ref} \cdot (b_0 + b_1 / 2 + n_2 / 4 + b_3 / 8) = -2 \cdot U_{ref} \cdot \frac{U_{4bit}}{16}

In this setup, we connect a binary code representing the desired voltage to the inputs of the resistor string, and an analog representation of the digital code is generated. It should be noted that the factor of 2 in the output voltage equation emphasizes that the output voltage in this configuration approaches 2 times the reference voltage (2xUref).

Standard resistors
----------------------------
This circuit is a lot more compact than the one before. For an N bit DAC it only needs N switches and N+1 resistors. Here’s the catch. Resistors don’t come in all shapes and sizes. Hop on to Mouser, Farnell, Digikey or whatever electronics supplier’s website you want and try to buy resistors for four bit binary weighted DAC.
If you select R value to be 10 kOhm, you wont’t have any problems with 2R, but you won’t find a 40 kOhm resistor. Closest you can get is 40.2 kOhm. How about 8R? Nope, 79.6 kOhm is your closest match. Depending on how precise you want to be, such tolerances are OK, but usually they aren’t.
Now let’s answer the question why can’t you buy any resistor you want? It has to do with tolerances. IF resistors are built with 10% tolerance, there is no reason to make standard values that are within 10% of one another. Manufacturers have agreed on making resistors in so called `E series <https://en.wikipedia.org/wiki/E_series_of_preferred_numbers>`_.
One way to alleviate the problem with tolerances is to use trimmable resistors, but this tends to be expensive. A smarter approach is to use a bunch of resistors from the same batch, even better if they were produced one after another, and hope they have the same error, which will then cancel out.

Experimenting with a binary weighted DAC
---------------------------------------------
With that out of the way, let’s use a bunch of 10 kOhm resistors to build a simple 3 bit binary weighted DAC. I have chosen to use 1V, generated by Red Pitaya, as a reference voltage. OpAmp is powered from +-3V. I opted to use switches instead of jumper wires for setting bits.

.. image:: img/13_binary_weighted_DAC_3b_schematics.png
	:name: three bit binary weighted DAC schematic
	:align: center

.. image:: img/13_binary_weighted_circuit.jpg
	:name: three bit binary weighted DAC circuit
	:align: center

A binary weighted DAC that we have just constructed uses the same number of resistors as the previous one, but it uses significantly fewer switches. That means lower parasitic capacitance, and thus higher maximum operation frequency.

R-2R
-------------
Instead of dedicating more time to obsolete or niche DAC architectures, let’s take a look at one that you can easily find used in commercial DACs. Here is how an R-2R resistor ladder looks like:

.. image:: img/13_R2R_schematic.png
	:name: R-2R DAC schematic
	:align: center

The R-2R resistor ladder is named after the two sizes of resistors it uses, R and 2R. The switches are omitted for simplicity in the diagram. In a practical implementation, the inputs marked with bits b0 through b4 would be connected to either GND or Uref. But how does this resistor ladder function as a DAC? The output voltage can be calculated as follows:

	.. math:: U_{out} = U_{ref} \cdot (b_0 /2 + b_1 / 4 + n_2 / 8 + b_3 / 16)

Or you can use superposition in conjunction with Thevenin’s theorem to verify. A harder alternative would be to use superposition and brute force, but I won’t try to stop you.
Unlike the binary weighted DAC that we explored before, a R-2R network does not need an amplifier to output a voltage. Keep in mind though that a buffer is still required for driving low impedance loads. In spite of that we might sometimes want to use an amplifier. If we selected an inverting amplifier, knowing the networks output resistance is essential. A nice property of a R-2R ladder: it “folds” down on itself.

.. image:: img/13_R2R_simplifications.png
	:name: R-2R DAC folding
	:align: center

As illustrated above, regardless of how many bits a R-2R network has, its output resistance is always R.
A nice thing about this DAC architecture is that we can easily add or remove bits simply by adding or removing a pair of resistors. As explained just a moment ago, this doesn’t affect the rest of the circuit. AS such, we can easily add an amplifier to the circuit. The following schematic depicts an inverting amplifier with a gain of -1 added to a R-2R DAC. Note how the amplifier consists of only an OpAmp and one resistor with resistance R.

.. image:: img/13_R-2R_with_inverting_amplifier_schematic.png
	:name: R-2R DAC with inverting amplifier
	:align: center

Another nice thing is that a R-2R DAC only requires N switches and 2N resistors of two sizes. By selecting R to be 10 kOhm, we can even obtain 2R (20 kOhm) from the standard set of resistors. Furthermore, R-2R ladders are so common that you can even buy a prebuilt network in a single component. They cost very little and may come in handy when we have enough free pins on a microcontroller. You don’t even need a special Uref, you can simply connect digital output pins to the inputs of a DAC and pretend that supply voltage is stable enough to serve as a reference voltage. Same holds true for a binary weighted DAC, but I see no reason why you would use that over a simple R-2R.

Experimenting with a R-2R DAC
--------------------------------------
Consider deriving a formula for R-2R DAC’s output voltage your homework and let’s quickly look at how we can construct a R-2R DAC. Just like before, I am taking Uref from Red Pitaya’s DAC, set to output a constant 1V signal.

.. image:: img/13_r2r_circuit.jpg
	:name: R-2R DAC circuit
	:align: center

An observant reader may have noticed that I chose to buffer the output voltage with an inverting amplifier with a gain of -1. As a result, the output voltage is inverted, but if you measure the R-2R's output, you'll notice that it's at zero volts regardless of how you set the switches (bits). This is because of how an inverting amplifier operates. If you disconnect the inverting amplifier or use a buffer instead, the R-2R will function as expected.

Extra credits
-------------------
This course did not include any screen captures of the constructed DACs' performance as analyzing horizontal lines can be uninteresting. However, you can witness the DACs in action by trying them out yourself. Additionally, you can also determine the output voltages of the DACs demonstrated in this article as the switches were labeled to make it easier to observe their state.

Conclusion
-------------------
And that brings us to the end. We set off to find a DAC that can be used for generating rapidly changing signals, which PWM could not handle. We explored three different architectures in order of rising complexity for understanding the principle of operation, but falling complexity of construction.
With that said, I hope you learned something. Bye! 

Written by Luka Pogačnik

This teaching material was created by `Red Pitaya <https://www.redpitaya.com/>`_ & `Zavod 404 <https://404.si/>`_ in the scope of the `Smart4All <https://smart4all.fundingbox.com/>`_ innovation project.
