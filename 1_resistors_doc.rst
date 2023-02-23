Resistor circuits
=================

Objective
-------------
The objective of this activity is to brush up on your existing knowledge about Kirchhoff’s laws and expand on that knowledge by showing how they can be applied in resistor circuits. A secondary outcome will be a preliminary understanding of the Red Pitaya STEMlab hardware and software - test & measurements applications.

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/i3624KeZ_tw" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Background
----------------
We will kick this lesson off by taking a look at the basic equation, you will need to know if you ever wanted to tinker with electronics.
  
  .. math:: U=R \cdot I


This equation is the backbone of resistor circuits. Note that “a resistor circuit” is every circuit that you leave untouched for long enough, if there are no active elements. The second equation that comes in handy in such circuits is the one, which describes power dissipation on any resistor.

  .. math:: P=U \cdot I

Even though you can always determine voltage drop across, and current flowing through any resistor, it often comes handy to remember two more equations, in which voltage drop or current is substituted for its function of the other two values.
  
  .. math:: P=I^2 \cdot R = U^2/R

With this out of the way, we can move on to…

Kirchhoff’s laws
---------------------
To solve a circuit that consists of more than one component, you will need to know two more things, also known as Kirchhoff’s laws.  Kirchhoff’s current law (KCL):

  *“The algebraic sum of currents in a network of conductors meeting at a point is zero. “*

To put it simply, any current entering a node must also leave it. If the first law talks about current, logic would suggest that he second will be about voltage (KVL).

  *“The directed sum of the potential differences (voltages) around any closed loop is zero.”*
If these concepts are still unclear, don't worry. We will clarify them through examples later on. Before that, we will explore some equations and facts that can simplify the process of solving resistor circuits.

Some equations and facts
-----------------------------

If there is only one voltage source in a circuit, it can be simplified using two equations for equivalent substitute resistors. This can help to easily calculate the voltage drop or current and make complex circuits easier to analyze.

  .. math:: R_{S_{series}} = R_1 + R_2

  .. math:: \frac{R_{S_{parallel}}}{1} = \frac{1}{R_1} + \frac{1}{R_2}

When you have a circuit with only one voltage or current source and one substitute resistor, you can simplify the circuit and then work backwards to calculate the branch voltages and currents. It's important to understand what happens to currents and voltages when you branch out. If the substitute resistor is split into parallel resistors, the voltage drop across them remains the same. In contrast, when the substitute resistor is split into series resistors, the voltage is divided among them proportionally to their resistance. This is due to the behavior of current, which remains unchanged when a substitute resistor is split into multiple series resistors. While this may sound complicated, knowing these principles can help you analyze complex circuits.

 .. math:: U_{R_x}=U_{R_{S_{series}}} \cdot \frac{R_x}{R_{S_{series}}}

This statement means that it is expected that when a resistor is divided into multiple parallel resistors, the current flowing through each resistor is divided in proportion to their resistance. Here are the equations that represent this relationship.

  .. math:: I_{R_x} = I_{R_{S_{parallel}}} \cdot \frac{R_{S_{series}} - R_x}{R_{S_{series}}}

It's not surprising that current prefers to flow through a path with lower resistance and the greater the resistance, the higher the voltage drop. Two more things to keep in mind when solving circuits in steady state are that capacitors act as an open circuit and inductors act as a short circuit.

Practical example
---------------------

The circuit we're going to use in this example is different from the one in the video, so you can solve it either by using Kirchhoff's laws or by substitution.

.. image:: img/1_Circuit_full.png
   :name: schematic of the circuit
   :align: center

Let's assume we need to calculate voltage drop, current, and power dissipation on a certain resistor. We can solve this problem by substitution. For parallel resistors, we can represent their equivalent resistance using the "|" symbol.

.. image:: img/1_simplifications.png
   :name: process of simplifying the circuit
   :align: center

Talking numbers, our goal is to calculate voltage drop, current, and power dissipation on a circuit. By using the substitution method, we did not have to calculate all voltage drops and currents. However, next, we will analyze the circuit using the more academic method. The circuit has two branching nodes, which means we will need two node equations (KCL). There are also three distinct current loops, and we will need one loop equation less (KVL).

  .. math:: I_0=\frac{U_0}{R_{S_{total}}} = \frac{U_0}{(R_1+(R_2 |(R_3+R_4))+R_5 )}=...

  .. math:: U_{R_2} = U_0 \cdot \frac{R_2 |(R_3+R_4)}{R_{S_{total}}} =...

  .. math:: I_{R_2} = \frac{U_{R_2}}{R_2} =...

  .. math:: P_{R_2} = U_{R_2} \cdot I_{R_2}=...

Note that there was no need to calculate all voltage drops and currents to reach our goal.
Next we will take a look at the more academic method. First we have to analyse the circuit. It has two branching nodes, which means we will need two node equations (KCL). We can also find three distinct current loops, and we will need one loop equation less (KVL).

.. image:: img/1_loops_and_nodes.png
   :name: loops and nodes
   :align: center

Let’s write them down.

  .. math:: A: \;\;\; I_2+I_3-I_1=0

  .. math:: B: \;\;\; I_5-I_2-I_4=0

I would like to mention that you should immediately see from the schematic that we have redundantly many currents. :math:`I_s`, :math:`I_1`, and :math:`I_5` are exactly the same, so are :math:`I_3` and :math:`I_4`.
Moving along the KVL loops, we must be adding any voltage that we hit from the + side, and subtracting those that we hit from the -.

  .. math:: L1: \;\;\; U_{R_1} + U_{R_2} + U_{R_5} - U_0 = 0

  .. math:: L2: \;\;\; U_{R_3} + U_{R_4} - U_{R_2} = 0

Let’s first take a look at what we can do with the two node equations. First we can substitute redundant currents in B with the ones from A:

  .. math:: I_5 - I_2 - I_4 = 0  \rightarrow  I_2 + I_3 - I_1 = 0

Keen eyed among you will notice that after this transformation, equations A and B are the same equation. That makes things easy as we can simply express one of the currents as a function of the other two and move on to solving voltage equations.

 .. math:: I_1 = I_2 + I_3
 .. math:: equation\;A

Voltage drops in voltage loops should be written as products of currents and respective resistances.

 .. math:: U_{R_3} + U_{R_4} - U_{R_2} = 0

 .. math:: I_3R_3 + I_3R_4 = I_2R_2

 .. math:: I_3(R_3 + R_4) = I_2R_2

 .. math:: I_2 = I_3\frac{R_3+R_4}{R_2}
 .. math:: equation\;B

This one wasn’t too bad, let’s take a look at the other voltage loop:

 .. math:: U_{R_1} + U_{R_2}+U_{R_5}-U_0=0

 .. math:: U_{R_1}+U_{R_2}+U_{R_5}-U_0=0

Unlike before, we are dealing with three distinct currents. This can be solved by plugging in :math:`equation\;A`, and we get:

 .. math:: (I_2+I_3)R_1+I_2 R_2+(I_2+I_3)R_5=U_0

 .. math:: I_2 (R_1+R_2+R_5 )+I_3 (R_1+R_5 )=U_0

 .. math:: (I_3  \frac{R_3+R_4}{R_2})(R_1+R_2+R_5 )+I_3 (R_1+R_5 )=U_0

 .. math:: I_3=\frac{U_0}{\frac{R_3+R_4}{R_2}(R_1+R_2+R_5 )+(R_1+R_5 ) }

And there you go, we now have an equation for :math:`I_3` that only relies on known constants. We only need to plug the values in and from there on, dominos will fall. Plugging :math:`I_3` into :math:`equation\;B`` yields :math:`I_2`. From there on, :math:`equation\;A` gives us :math:`I_1` and all of a sudden all currents are known. Lastly we can use :math:`equation\;L1` to get any voltage drop we desire and all left to do is to calculate the power, which is now one simple multiplication away.
Was this more difficult than doing substitutions? Depends on who you ask. We solved the circuit both ways and you chose the way that best suits you. Besides, the second method yields all voltages and currents at once, which is what you will usually tasked with on the exams.

Hands on
-------------

When working with circuits, it's common to use equations to solve for voltage, current, and power. In this experiment, we will be building a circuit with Red Pitaya and measuring voltage across resistors to test our calculations.

To get started, select resistors of your choice, but make sure they are not below 100 ohms to avoid any potential damage. Once you have your resistors, build the circuit on a breadboard as shown in the picture provided.

Now, you can choose the voltage source for U_0 from Red Pitaya's supply pins. You have the option to use 3.3 V, 5 V, or even -4 V.
  
.. image:: img/1_Extension_connector.png
   :name: Red Pitaya's pinout
   :align: center

With that done, you should hook the probes in 10x mode to Red Pitaya and fire up the oscilloscope app. Don’t forget to set the x10 attenuation in software as well! 
Since we are dealing with DC signals, you don’t need to hook up the alligator clips (they’re internally connected to Red Pitaya’s GND). You can now measure voltage on any node by connecting a probe to it.

.. image:: img/1_vezje.jpg
   :name: assembled circuit and hooked up board
   :align: center

One thing you might want to do, is to set up automatic mean measurements on both channels to make reading voltage easier (MEAS -> Operator = MEAN -> DONE).

.. image:: img/1_scope_cap_2.png
   :name: oscilloscope window
   :align: center

I encourage you to build a different circuit. Don’t exceed three branching nodes to keep the calculations simple. Try to calculate voltage drops and compare them with measured values.

Written by Luka Pogačnik

This teaching material was created by `Red Pitaya <https://www.redpitaya.com/>`_ & `Zavod 404 <https://404.si/>`_ in the scope of the `Smart4All <https://smart4all.fundingbox.com/>`_ innovation project.
