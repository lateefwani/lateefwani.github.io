
---
title: Combined optical-electrical-thermal simulations in CST
date: {}
permalink: /posts/2021/03/combining-simu/
excerpt_separator: <!--more-->
tags:
  - CST
  - Co-simulation 
published: true
---
Sometimes we required to evaluate the performance of a photonic device in which an electrical signal is applied to control the output of the device. This type of sitaution arises in the devices where the electrical signal is applied to heat the active material which in terns change the effective index of the material. Therefore, to analyze the complete performance of the device a combined simulation governing the three physical effects, optical, electrical, and thermal, is required. Due to involment the three  effects sometimes it becomes tricky to keep track of all the steps required to run the simulations perfectly. Sometimes it is possible that we miss one or two steps and in this case it will lead to the wrong results and wastage of time. So avoid these scenrio I have compiled here the steps that can easilied followed to setup a combined simulation in the CST. 

The three modules used here have different purposes and are used to evaluate different thysical parameters. In the electrical simulation, voltage and current distribution are obtained by solving continuity equation. The thermal model calculates the temperature distribution in the device due to imported electrical losses from the electrical simulation. In the optical simulation, the effective index change due to the thermal heating of the device is calculated by importaing the temperature distribution.

First we create the device model using high frequency simulation module and specify all the relevant parameters and the boundary conditions. Then, follow the steps mentioned here:

Fig.1 
Multi-physics electro-thermal coupling in the co-simulation module of the CST

Step - 1 Select the schematic view of project.

Fig_2.png

Step - 2  Select Tasks and from the drop down menu click on New Task.\\

Fig_3.png 

Step - 3 Now select Simulation project and click OK.

Fig_4.png

Step - 4  Then selet MWSSCHEM.

Fig_5.png

Step - 5 Click on 3D Model and after that click on create simulation project.

   Fig_6.png

Step - 5 A window as shown below will open, select the following options from the drop down menu as shown here. This will create an electrical simulation project. Here, we apply the voltage by defining the voltage port.
Fig_7.png

Step - 6 After this an electrical project will be created. In this project, boundary conditions, current or electrical ports are defined, and electrical conductivity of all the materials is inserted. The electrode material should be defined as the perfect electric conductor (PEC) otherwise the voltage can not be specified on the material.

Step - 7 To export the thermal loss term, in the post processing window select the Thermal Loss Calculation from the menu.
Fig_8.png

## Thermal Simulation
Step - 8 Electrical simulation task is complete and now we create the thermal simulation for this we go back to the step one i.e. schematic view of the original project.
Fig_2.png

Step - 9 Then select the New Task 
Fig_3.png

Step - 10 Select Simulation project and click OK.

Fig_4.png

Step - 11 Select MWSSCHEM.

Fig_5.png

Step - 12 Select 3D model and click on Create Simulation Project.

Fig_6.png

Step - 13 Select following  options as mentioned below for the steady state thermal simulations. For the transient thermal simulations select Thermal Transient instead of Thermal Steady State.

Fig_9.png

Step - 14 Now, define the boundary conditions, thermal properties of material, Temp. dependent refractive index and follow the next step to import field from electrical simulation.

Fig_10.png

Step - 15 In case of steady state solution, we define the time signal in excitation signal option by double clicking the default folder and selecting the fill the time according to our choice.

Fig_11.png


## Optical Simulation
Step - 16  Now, we create the photonic simulation project for this again we go back to the step one i.e. schematic view of our original project. The next four step are similar to the electrical and thermal simulation project.

Fig_2.png

Step - 17 Select as described below. 

Fig_12.png

Step - 18 After this, we import the thermal losses from the thermal simulation into the optical simulation by selecting field source as given below.
Fig_13.png

Step - 19 To extract temperature dependent optical properties we add temperature dependent epsilon values into the CST by import option.

Fig_14.png

After doing all this make sure that you have define the materials properties in all of the simulations steps for example electrical conductivity defined in the electrical simulation and thermal properties are defined in the thermal simulations similarly for the optical simulation. To run the co-simulation go to the schematic view of original project and click update. It will automatically run all the simulations.
