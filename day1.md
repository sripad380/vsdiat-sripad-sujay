# DAY 1

## How To Talk To Computers

### Introduction to QFN-48 Package, pads, chip, core, die and IP`s
A commonly used arduino board, the arduino leonardo, is shown below. It uses a chip which is just shown from high level view.
![arduino leonardo](https://github.com/user-attachments/assets/f2983e10-4bd6-48bf-a196-1a20efab5d50)

This can be represented via a block diagram as shown below
![image](https://github.com/user-attachments/assets/11eb7d67-ef18-40ae-b0e6-0f4aaff15cc2)

The picture represnts a large block called soc. This is commonly called a chip but in reality it is a package inside which the chip is present. One type of package commonly used in arduino boards is the QFN-48 package (**Q**uad **F**lat **N**o leads). A package is shown below.

![image](https://github.com/user-attachments/assets/3f481c5d-a0b6-48e0-bbf3-3e92df517f9f)

A chip usually is connected to many "**pins**" or I/O (inputs or outputs) sorrounding it. The position and placement of these pins are dependent on their PCB (**P**rinted **C**ircuit **B**oard). The chip has many complex structures within them such as :-
* pads
* die
* core
  * macro
  * IP`s

A schematic of all components is shown below :-

![image](https://github.com/user-attachments/assets/8efb5383-ef23-4209-9c6b-66a3d97e0b8c)


### Introdoction to RISC-V

RISC-V, is the language of the computer. When a program is to be run on a piece of hardware, it is first compiled in an assembly language program like RISC V. It is then converted to machine language i.e. binary and subsequently implemented in the form of a hardware description language such as picorv32 cpu core.

![image](https://github.com/user-attachments/assets/2f25247e-e849-4f9a-92c1-39fe8946bfb2)

### From Software Applications To Hardware

Applications run on hardware such as laptops, tablets etc that are powered by chips. However, apps are coded in complex programs that require conversion into binary code for running. This is done through system software which consists of :-
* Operatiing systems (OS)
* Compilers
* Assemblers

## SOC Design And Openlane

### Introduction to all components of open source digital ASIC

open souce digital ASIC requires mainly three things :-
* RTL IPs
* PDK Data
* EDA tools

Until june of 2020, there was no available OpenSource PDK. But then Google partnered with Skywater to release a 130nm open source PDK. there is a common thought that 130 nm chips are not upto date and chips can reach sizes of the devices themselves if used for the current generation chips. but 130 nm is good enough for smaller applications such as microcontrollers, microprocessors(which are not that powerful etc. Plus as an added bonus 130 nm is very fast and energy efficient due to the low resistance it offers.
![image](https://github.com/user-attachments/assets/d0af3ac1-25f2-4826-87bb-e66cf12d7bfd)

###simplified RTL to GDS flow

RTL to GDSII (**R**egister **T**ransfer **L**evel to **G**raphic **D**esign **S**ystem II) has many steps :-

* Synthesis - it converts hardware description languages such as VERILOG into gate-level representations part of a standard cell library. Each of these have different views/models such as HDL, Spice etc.
* floor planning - Floor Planning involves optimizing chip performance, area utilization, and connectivity. The three main purposes of floor planning are minimizing wire lengths, delays, optimizing power distribution, and ensuring efficient chip utilization.it includes various steps to be completed :-
     * chip floor planning - planning all the components of a chip
     * macro floor planning - planning all the components in a macro
     * powerplanning - planning all power rails
* placement - placement is to place the components in the most eficient way possible. It comes in two steps :-
     * global placement - placing in best positions without following any rules
     * detailed placement - placing all the components in the best positions given by global placement but making adjustments for following all rules.
* creating a clock distribution network - done first as propper timing with minimum differences is very important for any chip.
* routing - connecting components in the best way possible. it comes in two steps :-
     * global routing - connecting in best ways without following any rules
     * detailed routing - connecting all the components in the best ways given by global routing but making adjustments for following all rules.
* signoff - this process is just verification majorly through 3 processes :-
    * Design Rule Check (DRC) - this makes sure the design complies with manufacturing guidelines and is compatible for fabrication. It aims to detect and correct layout errors so that fabrication defects do not occur. 
    * Layout vs. Schematic (LVS) - here the layout is contrasted against the schematic to ensure consistency. LVS tools extract netlists and compare them for differences, after which the design proc eeds to physical design flow
    * Static Timing Analysis (STA) - it evaluates timing behaviour of a digital circuit to ensure design meets setup and hold time constraints, maximum clock frequency, and other timing requirements.

### Introduction to OpenLANE and Strive Chipsets

OpenLANE is an open-source digital ASIC jointly developed by efabless and Google, designed to automate the entire design process flow based on several open source components like OpenROAD, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. It aims to create a clean GDSII (i.e. no LVS violations, no DRC violations, and no timing violations) with no human intervention. striVe is family of SoCs created by Efabless using openlane.

![image](https://github.com/user-attachments/assets/0863a336-b2da-4dee-8b50-31ec56111171)

![image](https://github.com/user-attachments/assets/1c49062f-62e2-461f-9e58-2fa470ae53d4)

### Introduction to OpenLANE detailed ASIC Design Flow

![image](https://github.com/user-attachments/assets/d984fc07-baff-4cdf-9f23-472dec3cb488)

* Synthesis Exploration - it generates a delay x area report :-
![image](https://github.com/user-attachments/assets/acd66af2-a2bd-43c9-b948-a69f12e0d8de)

* Design Exploration - changes design configuration and subsequently find best configuration for any given design. :-
![image](https://github.com/user-attachments/assets/180e522f-bce7-4657-98d9-0037496d254f)

* OpenLANE Regression Testing :-
![image](https://github.com/user-attachments/assets/749d7dd4-d529-43cf-a6d6-a575b5ca9269)

* Design for Test - also called DFT :-
![image](https://github.com/user-attachments/assets/4f12aa0b-bd1f-4877-9b91-799e4bd05570)

* Physical verification (DRC & LVS) - Magic is used for DRC and Magic and Netgen for LVS.
* Logic Equivalence Check (LEC) - checks that the physical implementation and the netlist have the same logic. It is performed each time netlist is modified and checks that changing netlist did not change function.
* Dealing with Antenna Rules violations :-
![image](https://github.com/user-attachments/assets/3da9f1a8-bb3c-4d3a-9b9a-7a25a9d0926b)
![image](https://github.com/user-attachments/assets/d1d068b6-7975-4c0a-a4f2-a3ac78ceed43)
![image](https://github.com/user-attachments/assets/42c53bbf-763e-426f-9162-d6362a4a73b8)

* Timing Analysis (STA) - Here, the input also contains a synthesized netlist along with other data.

## Get Familiar to Opensource EDA tools

### OpenLANE Directory Structure in Detail

Openlane is more of a flow than a tool. The aim of openlane is to automate the entire RTL to GDSII flow and make it clean and opensource. It is very similar to commercial EDA tools.

Exploring OpenSource directory through Linux terminal steps:-


 Exploring OpenSource directory through Linux terminal steps:- 

 1. Open the virtual machine and then open terminal on it.
![image](https://github.com/user-attachments/assets/bf449a46-03f7-4698-8c42-76f1ac0636d0)
 
 2. type cd Desktop/work/tools
![image](https://github.com/user-attachments/assets/01a91b5a-f681-4ab8-8373-80b8f95e5b5e)

 3. type ls -ltr //we can see everything inside tools, ltr defines the order of the items.
![image](https://github.com/user-attachments/assets/ac20b708-1950-48f7-8086-f7dc0041b7b3)

 4. We are going to use openlane_working_dir, and so we will change directory to it by typing cd openlane_working_dir. Afterwards, we list the contents using the same ls -ltr.
 5. We are going to use pdks and openlane directories and so we will explore both in sequence.
 6. Type cd pdks and then ls -ltr to view the contents in pdks. Here, we will explore sky130A, so similarly change directory and then view contents. we see 2 directories : libs.tech, libs.ref
 7. libs.tech contains all tool specific files. As seen in the picture - we can see : (items given in image). (note :- to clear a page type clear)
![image](https://github.com/user-attachments/assets/6d110b67-7917-4791-b970-5d39c56fd65c)

 8. Opening the magic directory in libs.tech, we can see : (items given in image)
 ![image](https://github.com/user-attachments/assets/dd5f79ff-8ee9-4eaf-8e79-b70baef7bbe7)

 9. libs.ref contains all the technology/foundry related processes. Upon further exploration of sky130_fd_sc_hd, we see the following (cd ../ goes to the parent directory of the last file)
![image](https://github.com/user-attachments/assets/dbccc004-a5f5-4f88-a128-6244da3b01dc)
 
 10. next we open openlane :-
![image](https://github.com/user-attachments/assets/10d2ef9d-f278-41a6-8f71-c495e3a25f37)

### Design Preparation Step

To open Openlane, we can use the docker command.Then we type ./flow.tcl -interactive. The icon changes to a % symbol. Then we should type: package require openlane 0.9. thisretrives all the required information for openlane.

![image](https://github.com/user-attachments/assets/af750af7-de01-4d1c-a04f-364d2643fe94)

To setup the design of OpenLANE, we first need to prepare the design ensures that the final product functions correctly and reliably through **prep -design picorv32a** :-

![image](https://github.com/user-attachments/assets/b20a049c-8044-46c7-ae0e-e7e1fbbfbc1f)

The command when run, sets up a filesystem where the OpenLANE can store the results. This creates a folder inside the picorv32a directory which contains the command log files, results, and the reports dumped of the various tool. The folder will be only have the lef files generated by this design setup stage. The cell LEF files .lef and technology LEF files .tlef merge to generate merged.lef inside runs/tmp/, wherein a a folder with today's date will be created, inside which a tmp folder will have contents, and the merged.lef folder will contain the merged lef files.

### Review files after design prep and run synthesis

go to tmp and type **less merged.lef** the informaion on picorv32a is stored here :-
![image](https://github.com/user-attachments/assets/c5d87e4f-bf41-4328-beb3-74737780fdff)

config.tcl consists of specific values for many things in any design. go to the main folder(runs/date&time) and type **less config.tcl**:-

![image](https://github.com/user-attachments/assets/d5485085-4d55-4bd1-883c-4542c92972a4)

type run_synthesis. The synthesis will be run:-

![image](https://github.com/user-attachments/assets/d2e81cb2-5ea7-4438-89a1-e0bac12fb777)
