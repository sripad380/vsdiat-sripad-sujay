# SKY130 Day 2: Good vs Bad Floorplan and Introduction to Library Cells
## Chip Floor Planning Considerations
### Utilisation Factor and Aspect Ratio

The first step in physical design is to **_define the width and height of the core and die_** : Beginning with a very simple netlist, that can extrapolated later we will first draw a basic diagram in the form of symbols that we will later convert into physical designs. We will take each cell and give it a standard dimensions. As an example here, each unit will be 1 unit x 1 unit - i.e. 1 sq. unit in size, and since there are 4 gates/flip-flops here, the total size of the silicon wafer will 4 sq. units.

![image](https://github.com/user-attachments/assets/8747c15b-ae81-4f3d-8d4b-735714675bf4)

All logical cells will be placed inside the core. If the logical cells occupy the core fully, it is known as 100% utilisation. 

Utilisation factor = Area occupied by netlist / Total area of core. 

In this case, we see that utilisation factor is 100%, but practically it is usually 50%. Aspect Ratio is the ratio between height and width. If the chip is square - it is 1, else the chip is rectangular in shape.

![image](https://github.com/user-attachments/assets/857acedd-0125-4ee7-82f7-acefe053c15f)

![image](https://github.com/user-attachments/assets/d79603af-244e-4078-be5a-fc77e1920180)

![image](https://github.com/user-attachments/assets/0233efa7-fbd8-4a55-aee7-501199253781)

### Concept of Pre-Placed Cells

Pre-placed cells are reusable, complex logic blocks that are not modifiable by any tools afterwards and therefore require careful design. Their placement is user-determined. Combinational logic, such as the provided netlist, performs specific functions and consists of various gates. This logic can be divided into blocks while preserving connectivity. By extending IO pins and making necessary connections, the logic can be split into two black-boxed parts for reuse. Pre-placed blocks include memory, clock-gating cells, comparators, and MUXs.

![image](https://github.com/user-attachments/assets/61a40f0b-d5ce-44df-ab8d-fd6cb96dfecf)

![image](https://github.com/user-attachments/assets/763530e7-4d5d-4cf3-931e-7d87eb69a4f1)

![image](https://github.com/user-attachments/assets/7cbd3023-026f-48a6-8716-323668398d39)

![image](https://github.com/user-attachments/assets/b3641a2e-50f8-4db5-8fef-0c06703823cf)

### De-Coupling Capacitors

We use decoupling capacitors to stabilize important cells. When a circuit is powered on, it demands current from Vdd, and discharges to the ground when turned off. Due to resistance, inductance, and capacitance in the wires, the supplied voltage reduces slightly, resulting in Vdd. The Vdd needs to stay within a specific range for circuit stability, which can be problematic due to the distance between the voltage supply and the circuit. Decoupling capacitors maintain a voltage equivalent to the main supply, decoupling the circuit from the main supply. This ensures pre-placed cells receive stable power from the capacitors.

### Power Planning

when there are a lot of macro`s in any given circuit then they all will ant a particular voltage. but it is very inonvinient to add a lot of decoupling capacitors to a circuit. So we should add more than one voltage source to reduce the noise. We can also add a decoupling capacitor wherever there are more than one components requiring a decoupling capacitor. 

### Pin Placement and Logical Cell Placement Blockage

![image](https://github.com/user-attachments/assets/3e6733bb-1583-41f7-b91e-d31c8d610741)

The above netlist is an example needing to be implemented. The information about the connections is coded through VERILOG. The input and output ports are placed left and right between the core and the die respectively. The placements of the ports are in accordance to the cells. The clock ports are important as they continuously drive the cells so their ports have to be bigger than the data ports for lesser resistance. The size is inversely proportional to the resistance.

After the pin plcement, we create Logical Cell Placement Blockage to ensure that the APR tool does not place any cell on the pin locations.

![image](https://github.com/user-attachments/assets/ad692d25-f6b4-4a98-844a-98bd11195355)

### Steps To Run Floorplan Using Openlane

to run floorplan using openlane we have to type the command run _floorplan which will give this output:-

![image](https://github.com/user-attachments/assets/2436e4ad-4b26-4764-a8a4-7d33da4d620a)

### Review Floorplan Files and Steps to Review Floorplan

After running floorplan, it will store the result in the runs/(date and time)/logs/floorplan folder which will contain all results of the floorplan.

### Review Floorplan Layout in Magic

Type magic -T **/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &** to view the file. z is for zooming and ctrl +z is for unzoom. press s to select the whole die and v for centering it.

![image](https://github.com/user-attachments/assets/d827490d-4d37-42a3-a3b9-cca0c4ca12ab)


## Library Binding and Placement

### Netlist Binding and Initial Place Design

The first step is to bind the netlist with physical cells with real dimension. The netlist contains various gates that while in the schematic are of a certain shape are usually square/rectangular in shape in production. These gates are given a specific shape and look very different from the netlist.

These blocks are sourced from a place known as a library. The library has cells with various shapes, dimensions and delay. The library contains various sizes of cells with the same functionality too - since bigger cells have lesser resistance.

The second step is Placement, which is done based on connectivity. As can be seen, flip flop 1 is close to the Din1 pin and flip flop 2 is close to Dout1 pin. Combinational cells are placed in close proximity to FF1 and FF2 as to reduce delay.

### Optimise Placement Using Estimated Wire-Length and Capacitance

Here we estimate wirelength needed to connect the components together. If the wirelength is too long, we install repeaters to stop the signal from changing over a long distance. Repeaters essentially resend the same signal to it's prior strength.

### Congestion Aware Placement Using RePLACE

The command to run placement of OpenLANE - run_placement does three functions :-

* Global Placement - there is no legalisation and HPWL reduction model is used
* Optimization
* Detailed Placement - legalisation occurs - where standard cells are placed in rows and there will be no overlap of the cells.

After running the placement, output is generated in this folder **openlane/designs/picorv32a/runs/[design - date]/results/placement/picorv32a.placement.def**

Then, we can type the command : **magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &** to view it in Magic:

![image](https://github.com/user-attachments/assets/1356fcae-455b-46d2-9ac5-66e44d74ee3a)

## Cell Design and Characterisation Flows

### Inputs for Cell Design Flow

Standard cells - For example AND gate, OR gate, BUFFER etc are stored in the standard cell library. There are various types of cells in the library with various variations as well - in drive strengths, functionality, and voltages. For a greater cell size, there is greater drive strength for longer wires. If there is high Vth, then it will take more time to switch than a lesser threshhold voltage cell.

The standard cell design flow is as follows:-

* Inputs - PDKS : DRC and LVS rules, SPICE models, library and user defined specs

![image](https://github.com/user-attachments/assets/d5d69a4f-ffd6-4314-a1db-9521d865a050)

### Circuit Design Step

Process - circuit, layout design and charecterisation

![image](https://github.com/user-attachments/assets/7b6566a3-eea1-48db-9569-30028e7f062b)

### Layout Design Step

Output - Circuit Description Language, GDSII, lef, timing, noise etc

![image](https://github.com/user-attachments/assets/9edeec58-8e0a-4f99-854e-f2794c3cc05d)

### Typical Characterisation Flow

Steps of Characterisation Flow:-

* Reading of SPICE module files
* Reading of netlist extracted by SPICE
* Recognising buffer behaviour
* Reading subcircuits
* Attaching neccessary power sources
* Applying stimulus
* Provision of of neccessary output capacitance
* Provision of simulation command

These steps are given to the to a software called GUNA in the form of a configuration file, which will generate timing, noise and power models in the form of .libs files.

## General Timing Characterisation Parameters

### Timing Threshhold Definitions

We will talk about the various .libs files generated by GUNA. To do this, we will take this circuit as an example:

![image](https://github.com/user-attachments/assets/401c7e7d-9cbe-4be3-aa3e-4dcfd92c0d1a)

Here, the red line is output of first inverter and blue is output of second inverter.

![image](https://github.com/user-attachments/assets/c5ab1ca5-e4cc-4989-8934-9a4aca9d5ec9)

(exception : we will be taking a single inverter for this example)
![image](https://github.com/user-attachments/assets/054f8fdc-9eb8-4211-8f1a-5fb41383d41d)

![image](https://github.com/user-attachments/assets/06098372-b233-4faa-ac25-a22e2faebed1)
![image](https://github.com/user-attachments/assets/210e575c-e9e4-48ed-91d1-a21b79542c22)

### Propogation Delay and Transition Time

Propogation delay is calculated as = time(out_x_thr) - (time_x_thr). If the propogation delay is negative, it can cause quite unexpected results - as an output is generated before the input. Hence, threshhold values should be selected properly. Delay threshold is usually 50% and slew rate threshold is usually 20%-80%.

Transition time is calculated as = time(slew_high_x_thr) - time(slew_low_x_thr)

![image](https://github.com/user-attachments/assets/61060c2c-a3e7-47ff-81f2-cdef663c6141)

