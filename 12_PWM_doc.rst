Pulse Width Modulation
============================

Introduction
-----------------
In the previous course, we briefly touched upon the concept of DAC or Digital to Analogue Converter, which converts digital values into analogue ones, usually voltage or current. Its usefulness is apparent whenever there is a need to convert digital signals to analogue values. However, in this course, we will be focusing on Pulse Width Modulation (PWM), which can be used to generate a stable voltage.
PWM is a familiar tool to most readers, and it is widely used for dimming light sources, adjusting power of small motors, and various other applications. It operates by supplying power to the device only a fraction of the time, thus reducing the average output power. If the repetition cycle is fast enough, the device's output power is perceived only as the average output power. In this course, we will explore how PWM is generated, when it can be used, and how it can be improved.


.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/Vh6ioDh9yO0" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>





Digital PWM
------------------
In microcontrollers, PWM is typically implemented using a counter with a fixed size. The counter is incremented by one on each clock cycle, and when it reaches its maximum value, it overflows and resets to zero, causing the output to be set to a high voltage level. This counting process continues indefinitely. When the counter matches a reference value, the output is switched to a low voltage level. By varying the reference value, it is possible to adjust the output waveform and achieve the desired pulse width modulation.

.. image:: img/12_PWM.png
	:name: digital PWM
	:align: center

As depicted in the above image, changing the reference value (blue) affects the pulse width (red). However, there are two extreme cases to consider: when the reference is set to zero and when it is set to the maximum value. It is not possible for the PWM output to reach 100% duty cycle, as the counter will always match the reference value at the maximum value. Conversely, the output can easily reach 0% duty cycle, even though it is supposed to be set high when the counter overflows.

It is important to note that digital PWM modulators have a finite resolution, which is determined by the size of the internal counter. For instance, if the counter has four bits, as shown in the above image, the modulator can produce sixteen different pulse widths (2^4). Similarly, an eight bit counter would allow for 256 possible pulse widths, and so on.

Problems with fast response time
-------------------------------------
Pulse Width Modulation (PWM) is typically utilized in systems where the response time is slow compared to the PWM modulation frequency. For instance, dimming LEDs is an ideal use case since the human eye has a low "framerate". In this experiment, we will use the Red Pitaya's built-in PWM generator instead of building an analog PWM modulato
Set the Red Pitaya to output a PWM at full amplitude on output1 and connect an LED with a 100 Ohm series resistor to it. To avoid the need to amplify the 1V amplitude signal, we can set the second output channel to output DC voltage at 0V and offset of -1. Connect this to the LED instead of GND.

.. image:: img/12_PWM_LED_circuit.jpg
	:name: digital pwm circuit
	:align: center

To verify that the system is working, change PWM setting and see if LED’s brightness changes. Once done, try changing the modulation frequency. Nothing should happen at high frequencies, but you will notice blinking if you go too low. At that point you will have experience the problem that PWM causes when sensing devices have a fast response time. Digital cameras are even more sensitive to flickering than you are.

An unexpected experiment
---------------------------------
By setting up the Red Pitaya with the LED and a 100 Ohm series resistor as described earlier, you can conduct an experiment to test whether the human eye can see blinking at a 30 Hz refresh rate with a 50% duty cycle. However, it should be noted that the blinking will be easily noticeable unless the duty cycle is above 90%, in which case the off period is too short to detect. This raises the question of how old films could be shot at 24 Hz without causing noticeable blinking. The answer is that when projecting the film, the same image was flashed three times, creating an apparent refresh rate of 72 Hz, which was sufficient to trick the human brain into perceiving smooth motion. In fact, the brain can even reconstruct smooth motion at lower frame rates.

To verify that the LED is indeed modulated with PWM, you can move it rapidly and observe the spots produced along its trajectory. An interesting fact is that our peripheral vision is more sensitive to pulsating light, which can be tested out as well.

Efficiency
---------------
Despite the potential issue of flickering, PWM has many advantages that make it a popular choice in various applications. One major advantage is its high efficiency. By rapidly switching the signal between high and low levels, the resistive losses are minimized as the output transistors are fully opened or closed. This is in contrast to an OpAmp's output stage where the voltage difference between the supply voltage and output pin is wasted on output transistors, resulting in significant power losses at high load currents. It is important to note that power loss is the product of voltage drop and current. Therefore, by minimizing voltage drop, PWM can greatly reduce power losses and increase efficiency.

Analog PWM
-----------------
Analog PWM is generated in much the same way as a digital one. We need a rising voltage, that periodically resets (a sawtooth waveform), a reference voltage, and a way to compare them (a comparator). If reference is above “counter” voltage, output is high and vice versa.

.. image:: img/12_analog_PWM.png
	:name: analog digital PWM
	:align: center

We can construct such modulator. Instead of building an analog sawtooth generator, we’ll be using Red Pitaya’s function generator.
Sawtooth is fed into the comparator’s inverting input, and the reference voltage is fed into the noninverting one. Reference can be generated by using a potentiometer, or, as I did, by using Red Pitaya’s other output channel to output a DC voltage. From this point on, we have a fully functional analog PWM modulator.
Just for fun, let’s connect an LED to the output and give it a go. Schematic:

.. image:: img/12_analog_PWM_schematic.png
	:name: analog PWM schematic
	:align: center

The circuit:

.. image:: img/12_analog_PWM_circuit.jpg
	:name: analog PWM circuit
	:align: center

And a screen capture:

.. image:: img/12_analog_PWM_screencap.png
	:name: analog PWM screen capture
	:align: center
There is no inherent requirement to use a DC signal as a reference voltage for PWM modulation. In fact, other waveforms, such as sine waves, can be used. However, there is a limitation to the amplitude of the sine wave. It must be less than the amplitude of the sawtooth waveform to ensure proper modulation without clipping. In this particular experiment, a sine wave with an amplitude of 0.45 V and a DC offset of 0.5 V was chosen as the reference voltage.

.. image:: img/12_analog_PWM_sine_screencap.png
	:name: analog PWM sine wave modulation screen capture
	:align: center

This screen capture is representative of how a PWM modulated sine wave should look like, but you wouldn’t use it like this realistically. PWM carrier frequency has to be way higher than signal’s frequency. Setting sine wave frequency at 1 Hz achieves a pleasing result.

Smoothing the signal
-------------------------
As demonstrated in the previous experiment, a 100 Hz PWM signal is sufficient to deceive the human eye into perceiving a constant light. However, in certain applications such as lighting for movie sets or setting a threshold voltage for a comparator, a higher PWM frequency may be required. Although PWM is a cost-effective solution, increasing the frequency and implementing signal filtering using an RC filter may lead to significant cost savings. It is crucial to note that the time constant of the filter must be much greater than that of PWM for adequate smoothing. Additionally, if the filtered PWM signal is connected to a load with a low impedance, it must pass through a voltage buffer (OpAmp follower) to prevent signal degradation.

In this scenario, we can simplify the circuit by removing the LED and output2. We can connect an RC filter across the output1's connectors, and input1 can be set to 10x and linked to the output of the RC filter.

.. image:: img/12_PWM_filtering_circuit.jpg
	:name: smoothing PWM signal
	:align: center

And for clarity's sake, here's a schematic:

.. image:: img/12_PWM_with_RC.png
	:name: smoothing PWM signal schematic
	:align: center

Depending on PWM frequency you will get different results. I used a 10 kOhm resistor and a 1 uF capacitor. Here is what I got at 100 Hz PWM:

.. image:: img/12_PWM_RC_100Hz_screencap.png
	:name: PWM smoothing at 100Hz
	:align: center

At 1 kHz:

.. image:: img/12_PWM_RC_1kHz_screencap.png
	:name: PWM smoothing at 1kHz
	:align: center

And here is the result at 10 kHz PWM:

.. image:: img/12_PWM_RC_10kHz_screencap.png
	:name: PWM smoothing at 10kHz
	:align: center

For an RC filter with a time constant of about 15 Hz, 10 kHz input may seem to be a bit much, but that is what it takes to completely smooth out a PWM. And don’t worry, this frequency isn’t even on the high side. For an eight bit PWM, input clock has to be only 256 kHz to achieve a modulation frequency of 10 kHz, which is very low. It might be worth considering increasing the frequency even further and using a smaller capacitor. 
The filtered signal that we obtained is steady enough to use as an input to a comparator. Problems appear when we try to change the voltage. With the RC filter having a 15 Hz cutoff frequency, we obviously can’t change output signal faster than this. Filtered PWM is therefore only usable for generating very slowly changing signals. We will explore alternatives for generating higher frequency signals in the next course.

Conclusion
-----------------
And this brings us to the end. We’ve looked at PWM, how it works, its benefits, and drawbacks. We even discussed human physiology for a moment. I hope you found this course interesting and maybe even useful for one of your projects. Until next time, cheers!

Written by Luka Pogačnik

This teaching material was created by `Red Pitaya <https://www.redpitaya.com/>`_ & `Zavod 404 <https://404.si/>`_ in the scope of the `Smart4All <https://smart4all.fundingbox.com/>`_ innovation project.
