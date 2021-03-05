---
title: Combined optical-electrical-thermal simulations in CST
date: {}
permalink: /posts/2019/06/combining-pdfs/
excerpt_separator: <!--more-->
tags:
  - references
  - bash
published: true
---
Sometimes we required to evaluate the performance of a photonic device in which an electrical signal is applied to control the output of the device. This type of sitaution arises in the devices where the electrical signal is applied to heat the active material which in terns change the effective index of the material. Therefore, to analyze the complete performance of the device a combined simulation governing the three physical effects, optical, electrical, and thermal, is required. Due to involment the three  effects sometimes it becomes tricky to keep track of all the steps required to run the simulations perfectly. Sometimes it is possible that we miss one or two steps and in this case it will lead to the wrong results and wastage of time. So avoid these scenrio I have compiled here the steps that can easilied followed to setup a combined simulation in the CST. 

The three modules used here have different purposes and are used to evaluate different thysical parameters. In the electrical simulation, voltage and current distribution are obtained by solving continuity equation. The thermal model calculates the temperature distribution in the device due to imported electrical losses from the electrical simulation. In the optical simulation, the effective index change due to the thermal heating of the device is calculated by importaing the temperature distribution.

First we create the device model using high frequency simulation module and specify all the relevant parameters and the boundary conditions. Then, follow the steps mentioned here:

Fig.1 
Multi-physics electro-thermal coupling in the co-simulation module of the CST

## Electrical Simulation

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






<!---How many times have you found that your institution has access to a digital version of a book you need only to discover that it comes in 15 different PDF files?
<!--more-->
<!---I use [Zotero](https://www.zotero.org/) as my reference manager and it's difficult to attach more than one file to an entry, so I've certaintly spent time in the past painstakingly combining every section of a book together before adding it to Zotero. I finally got tired of doing this by hand and wrote a short Bash script to automate this process. Now I can combine as many PDFs as I want with a single command!

# How it works

My solution relies on [Ghostscript](https://www.ghostscript.com/) to combine multiple PDF files from the command line. On a Mac you can easily install Ghostscript using [Homebrew](https://brew.sh/) by running

```bash
brew install ghostscript
```

Once you've done that you've got everything you need. Create a shell script and put the following in it

```bash
#!/bin/bash
if [[ $# -eq 0 ]]; then
  gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=output.pdf *.pdf
else
  gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=$1 *.pdf
fi
```

The script first checks if you've supplied an output filename as an argument, and if not uses the default name of `output.pdf`. Be aware of where you run this script as it will overwrite a file called `output.pdf` if it exists in the active directory! If you have supplied an output filename, it will use that instead of `output.pdf`.

## Running it

To actually run this script, there are two steps left:

1. Make it executable
2. Add it to your PATH

Making your script executable is relatively straightforward. Use the `chmod +x` command in Terminal on your script. I saved mine as `PDFcombine.sh` so I ran `chmod +x PDFcombine.sh`. Putting your script on your PATH just ensures that your Terminal will be able to find it when you call it to combine PDF files. It's a little more complicated so I'll just link to this [excellent StackExchange answer](https://unix.stackexchange.com/a/26059) on how to do so. On my system this script lives in a directory in my Dropbox with similar other small utilities called `Bash`, so my `.bash_profile` has these lines in it

```bash
# custom bash scripts                                                           
export PATH=$PATH:/Users/Rob/Dropbox/Methods/Bash
```

to add it to my PATH. As an added bonus Ghostscript will (usually) rotate any pages containing sideways tables or figures!

# A warning

This script will combine PDF files in the order that `ls *.pdf` returns them. By default, this will be an *alphabetic sort*, so the files 1.pdf, 2.pdf, and 10.pdf would be combined in the following order: 1.pdf, 10.pdf, 2.pdf.

You can fix this by adding leading zeroes to all filenames so that each ID string is the same length. Most digital versions of books give you filenames like this, but be sure to check, otherwise your combined PDF might require a lot of skipping around. This script can be written to perform a *natural sort* of input files, but the code to do so is more complex, so I'll cover it in a future post.
