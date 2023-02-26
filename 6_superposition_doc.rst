Superposition
================

Introduction
------------------
Let's take a break from OpAmps, filters, and other complex circuits and focus on a fundamental technique used in linear circuits with multiple voltage or signal sources: superposition.

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/emmXle4LSbc" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

But why?
----------------
The answer is simple. Take this very simple circuit and tell me, what current flows through, let’s say… R2?

.. image:: img/6_schematic.png
	:name: a simple circuit
	:align: center

Even though this circuit consists of mere five components, solution is all but trivial. In the first course I showed you how to simplify a simple resistive circuit, but this doesn’t work here as we have two voltage sources. The only way to solve this circuit is by good old fashioned calculations. Let’s start with the two nodes.

	.. math:: A:-I_1+I_2-I_3=0 \rightarrow I_2=I_1+I_3

Ok, this wasn’t so bad, how about the loops?

	.. math:: L1
	.. math:: U_{R_1}+U_{R_2}-U_1=0
	.. math:: I_1 R_1+I_2 R_2=U_1
	.. math:: I_1 R_1+(I_1+I_3 ) R_2=U_1
	.. math:: I_1 (R_1+R_2 )+I_3 R_2=U_1
	.. math:: I_3=\frac{1}{R_3} (U_1-I_1 (R_1+R_2 ))
	
	.. math:: L2
	.. math:: U_{R_3}+U_{R_2}-U_2=0
	.. math:: I_3 R_3+(I_1+I_3 ) R_2=U_2
	.. math:: I_3 R_3+(I_1+I_3 ) R_2=U_2
	.. math:: I_1 R_2+I_3 (R_2+R_3 )=U_2
	.. math:: I_1 R_2+\frac{R_2+R_3}{R_3}  (U_1-I_1 (R_1+R_2 ))=U_2
	.. math:: I_1 (R_2-\frac{(R_2+R_3)(R_1+R_2)}{R_3})=U_2-\frac{R_2+R_3}{R_3}  U_1
	.. math:: I_1 = \frac{U_2-\frac{R_2+R_3}{R_3}  U_1}{R_2-\frac{(R_2+R_3)(R_1+R_2)}{R_3}}

Luckily this is all we need. Let’s say that all resistors were 100 ohms, U1 was 5V and U2 was 3.3V. Plugging those numbers in above equations we get the following resistor currents:

	.. math:: I_1=22.3 mA
	.. math:: I_2=27.7 mA
	.. math:: I_3=5.3 mA

Since all resistors are 100 ohms, the voltage drops on resistors are just current through a resistor times one hundred.

A better alternative
---------------------------
If you skipped all the calculations in the last chapter, I don't blame you. They were anything but fun. And that was for a very simple circuit with two voltage sources and three resistors. Just imagine what would happen if we had even more components!

Fortunately, there is a better way: superposition. If a circuit is linear (meaning that the output can be written in the form of :math: 'a * U1 + b * U2 + ... + x * I1 + y * I2 + ... = X'), we can analyze the circuit with only one source connected at a time. We do this for all sources and then sum up the results to get the full output. This is known as superposition.

Note that "disconnection" means something different for voltage and current sources. A voltage source is replaced by an open circuit, while a current source is replaced by a short circuit, as shown below.

.. image:: img/6_disconnection.png
	:name: correct disconnection modes
	:align: center

Let’s see how one would apply superposition theorem on this circuit. First, let’s disconnect U2 and solve the resulting circuit. Mind the direction in which currents are flowing as this is very important.

	.. math:: I_{1_{(1)}}=\frac{U_1}{R_1+R_2 |R_1}=\frac{5 V}{100 \Omega+50 \Omega}=33.3 mA
	.. math:: I_{2_{(1)}}= -I_{3_{(1)}}=\frac{I_{1_{(1)}}}{2}=16.7 mA

When U1 is disconnected we get the following:	

	.. math:: I_{3_{(2)}}=\frac{U_2}{R_3+R_1 |R_2}=\frac{3.3 V}{100 \Omega +50 \Omega}=22 mA
	.. math:: I_{2_{(2)}}= -I_{1_{(2)}}=\frac{I_{3_{(2)}}}{2}=11 mA

And to get the final result it up:

	.. math:: I_1=I_{1_{(1)}}+I_{1_{(2)}}=33.3-11=22.3 mA
	.. math:: I_2=I_{2_{(1)}}+I_{2_{(2)}}=16.7+11=27.7 mA
	.. math:: I_3=I_{3_{(1)}}+I_{3_{(2)}}=-16.7+22=5.3 mA

If you ask me, this method is a lot better. Much simpler. Harder to get wrong. Add more positive descriptors.

The expereiment.
-----------------------
There is always an experiment. But this one will be extra simple. Build a circuit and learn how to efficiently measure it. 5V, 3.3V, and GND are stolen from the Red Pitaya and both probes are set to 10x mode.

.. image:: img/6_experiment.jpg
	:name: experimental setup
	:align: center

As this circuit operates solely on DC signals, channels 1 and 2 will remain constant, effectively serving as voltage meters. To calculate the voltage drop between nodes, select MATH->Operator = “minus“->ENABLE. Additionally, it is recommended to add automatic measurements to all signals by clicking MEAS->Operator = “MEAN” ->DONE. This should be done for IN1, IN2, and MATH signals. With these measurements set up, the circuit can be analyzed and manipulated as desired. For those interested in experimenting with more complex circuits, it is worth noting that Red Pitaya offers an additional voltage output pin, -4V, which can be utilized in circuit design.

.. image:: img/6_meas.png
	:name: measurement
	:align: center

Conclusion
------------------------
Superposition is a valuable technique for analyzing linear circuits, providing an easier alternative to standard calculations wherever possible. To use superposition, we disconnect all but one source and calculate the output, then repeat for the other sources and sum up the results to get the final output. In the next course, we will explore a practical application of superposition.

Written by Luka Pogačnik
Editted by Andraž Pirc

This teaching material was created by `Red Pitaya <https://www.redpitaya.com/>`_ & `Zavod 404 <https://404.si/>`_ in the scope of the `Smart4All <https://smart4all.fundingbox.com/>`_ innovation project.
