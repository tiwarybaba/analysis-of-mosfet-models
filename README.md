 # Analysis of mosfet models
 ## General MOS Analysis
 This section startst with our analysis of MOSFET models present in sky130 pdk. I would be using the 1.8v transistor models. below is the schematic I created in Xschem.
 ![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/7295b3a0-2d07-4acd-adb2-716f61ffcf35)
 The components used are:
nfet_01v8.sym - from xschem_sky130 library
vsource.sym - from xschem devices library
code_shown.sym - from xschem devices library

I used the above to plot the basic characteristic plots for an NMOS Transistor, That is Ids vs Vds and Ids vs Vgs. To do that, just save the above circuit with the above mentioned specifications and component placement. After this just hit Netlist then Simulate. ngspice would pop up and start doing the simulation based calculations. It will take time as all the libraries need to be called and attached to the simulation spice engine. Once that is done, you need to write a couple commands in the ngspice terminal:

display - This would display all the vectors available for plotting and printing.
setplot - This would list all the set of plots available for this simulation.
after this choose a plot by typing '''setplot <plot_name>'''. for example '''setplot tran1'''
plot - to choose the vector to plot.
example : plot -vds#branch

Then you must see the plot below you, if you did a DC sweep on the VGS source for different values of VDS:
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/da77346b-584a-45b7-802b-8cf7d4bc53fd)
This definitely shows us that the threshold value is between 600mV to 700mV and I think I will be using 650mV for my future calculations. Similarly, when I sweep VDS source for different values of VGS, I get the below plot:
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/1419528e-586b-450d-a640-f29d72ee31f7)
Now the above two definitely looks like what the characteristics curves should, but now we need to choose a particular curve that we would do further analysis on.

I also did plot gm and go(or ro) values for the above mosfet. This would be crucial as we can obtain a lot of parameters from these values. Both of these below are for the general dc sweep we did above.
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/b279e2d3-5d95-44a9-ab03-1cec63b96998)
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/84687bca-7d53-4cfd-9bc5-a7f1506f4e04)
Since I am making an inverter, let's choose the highest value avialable for the Vds, that is 1.8V. So to do that, we just change the value of Vds source to 1.8 and then hit netlist, then simulate to simulate the circuit.
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/3ec701d6-6c6d-467f-985a-e910be4fcfd2)
This above plot also tells us the value of current at this value of Vgs which is around 320uA. Next step is to calculate the Gm, that is the transconductance parameter. To do that we simple type the commands as shown in the console in the left hand side. The deriv() function takes the derivative with respect to the independent variable present at the current simulation. From the definition of Gm we are aware that it is dIds/dVds. Hence, I did the same to plot the Gm of this nfet. After this I measured the value at 1.8V.
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/57012a31-8aee-4c90-b230-c5dfb8373a86)
Similaraly I did the same for Ids vs Vds and also used that to find rds
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/6523fcfa-979f-47d2-821a-1c57f91cddd0)
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/6100f2a9-8ff6-4fba-ae60-9a1db02f307e)
Hence, we now have all our important values we needed. Same can be done for a PMOS. Motive is same, but expecially to extract the value of Aspect ratio for which the current is the same in both NMOS and PMOS. I have done some experimentation and found that at W/L of PMOS = 4 * (Aspect ratio of NMOS), the current value is pretty close. So, we found the NMOS had a current of 317 microamps while PMOS has the current of 322 microamps (both at |Vgs| = 1.8V). SO like 5 microamps apart.
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/5248e86a-812d-458f-acf8-7650e23792c0)
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/7c86b22f-c17a-4708-9b55-ac6ba15a47df)
##  Strong 0 and Weak 1
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/f91e6540-5432-417a-b8c6-28ed35cd1d8e)
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/53e93e79-c898-47cf-8f65-f52cb9c489c3)
You can see that, when a square wave is applied to the input of NMOS, when it is LOW(0V), the output goes to HIGH(1.8V). But when the input is HIGH(1.8V), the output goes to a value that is much larger than 0V. This is due to the fact that when Vgs is 1.8V, the NMOS is in linear region. This is where the MOSFET acts as a voltage controlled resistor. At this point, the output is connected to a Voltage Divider Configuration. That is the output takes the value which is defined by the voltage across the resistance of the mosfet. Hence, NMOS is able to transmit STRONG 0, but not a STRONG 1. So NMOS is Strong 0 but a Weak 1

##  Weak 0 and Strong 1
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/640c4012-bb26-4f6a-bb0e-5ee3d3203544)
![image](https://github.com/tiwarybaba/analysis-of-mosfet-models/assets/117029388/34c8c1e1-351b-4ca8-8e37-093610ff7ecc)
### Hence, neither NMOS nor PMOS would make a great inverter on their own. So a plethora of configurations were taken into account, but at the last, only one stands as the most popular format of circuit design using mosfets. It is referred to as a CMOS configuration















 
