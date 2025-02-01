# SKY130 DAY 3: Design Library Cell Using Magic Layout and NGSPICE characterisation
## Labs for CMOS Inverter NGSPICE Simulations
### IO Placer Revision

OpenLANE configurations can be changed inside the shell itself. IO Mode is usually set to _random equidistant_  [env(FP_IO_MODE)_1]. However, if we want to change this, we can do so through the following command typed after floorplan: **set ::env(FP_IO_MODE) 2**. After running this command, the IO input / output pins will not be equidistant in mode 2 (instead of the default - that is 1). 

After this, we may re-run floorplan, and then check by seeing that the pins are placed based on of Hungarian algorithms now stacked one over the other. 

![image](https://github.com/user-attachments/assets/9c7156db-e377-45cf-af1e-adc8eee8b4d5)

### SPICE Deck Creation For CMOS Inverter

The SPICE deck contains connectivity information about netlists, inputs to be provided, TAPS for the outputs etc. The component values are taken, that are usually :-

* for the PMOS AND NMOS it is .375u/.25u (i.e. the channel length is .25 micron and and the channel width is .375 micron).

![image](https://github.com/user-attachments/assets/4b4afc64-1a45-4226-830c-040a4b7eff52)

### SPICE Simulation Lab for CMOS Inverter

The syntax of the SPICE deck netlist PMOS and NMOS is [component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]. It is to be noted that all components in a netlist are described based on its node and values.

![image](https://github.com/user-attachments/assets/361f7be0-ab29-42f8-b316-c9cf0f7c50e5)

1st line
* M1 -> component name
* out -> output
* in -> input
* vdd -> source
* vdd -> substrate
* PMOS -> transistor type
* W=0.375u -> width
* L=0.25u -> length

2nd line
* M2 -> component name
* out -> output
* in -> input
* 0 -> source
* 0 -> substrate
* NMOS -> transistor type
* W=0.375u -> width
* L=0.25u -> length

![image](https://github.com/user-attachments/assets/0c433b21-0422-42bf-8139-ba11393ca1e9)

the green line is output for opposing input

![image](https://github.com/user-attachments/assets/880af982-5994-43eb-9390-0d8a392c1cbc)


### Switching Threshold Vm

![image](https://github.com/user-attachments/assets/492c7281-14a2-48d1-bf21-68e32aa83350)

Note:

* The shapes of the graphs are almost the same, so we can conclude that CMOS is a robust device
* The parameters that determine the robustness of the CMOS is the switching threshold and the propogation delay
* The Switching Threshhold is the point where the the input voltage is equal to the output voltage.
* Vm is the point where Vin=Vout

### Static and Dynamic Simulation of CMOS Inverter

To find Vm, we use dc transfer analysis. Simulation is essentially a sweep from 0V to 2.5V by taking 0.05V steps.

![image](https://github.com/user-attachments/assets/8fc3ef47-fdad-4056-bf4a-91ba1f1630e3)

To find propogation delay, we use transient analysis when a pulse is applied to the CMOS.

![image](https://github.com/user-attachments/assets/2af5f996-0722-4351-bc43-1e64dbcb1625)

![image](https://github.com/user-attachments/assets/37375b44-2fcb-4f5d-adc0-8361f70d170a)

### Lab Steps to GitClone VSDSTD Cell Design
A github repository was provided where the inverter files lie. It is available at the link - https://github.com/nickson-jose/vsdstdcelldesign . Steps to clone and observe the layout are as follows:

1. Clone the repository with the custom inverter design through the command git clone https://github.com/nickson-jose/vsdstdcelldesign
2.Subsequently, copy the tech file to the vsdstdcelldesign directory by the command cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
3. Then, open the custom inverter layout in MAGIC through this command: magic -T sky130A.tech sky130_inv.mag &

![image](https://github.com/user-attachments/assets/e56ec8bd-0640-4058-956f-ce998f0375f5)

## Inception of Layout and CMOS Fabrication Process

### Create Active Regions

1. The first step is to select a substrate where the entire design is fabricated. The most common substrate is a P doped Silicon Substrate. A substrate is ideally lesser doped than it's wells.

2. The next step is creating an active region for transistors. It is to be noted that it is necessary to have isolation between the pockets, which can be done through
   * Growing 40nm of Silicon Dioxide
   * Depositing 80nm of Silicon Nitride
   * Depositing a layer of photoresist
   * Deposit mask-1 layer on top of photoresist. It covers the photoresist layer that must not be etched away (protects the two transistor active regions)
   * Applying UV light to remove the layers on the unmasked regions
   * Removing mask-1 and photoresist layers
   * Placing the chip in the furnace to grow the oxide in other areas
   * Removing the Si3N4 layer using hot phosphoric acid to have only p-substrate and SiO2 left

### Formation of N and P well

3. P well and N well formation
   * Deposition of photo resist layer and define the areas to protect by deposition of mask-2 and 3. Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated and Mask 3 protects P-Well while N-Well is being formed
   * Application of UV Light to remove the exposed photoresist
   * Placing of chip in furnace to diffuse the boron and phosphorous to form wells. This process is called Twintub process.

### Formation of Gate Terminal

Gate Terminal is where Threshhold Voltage is controled - as seen below:

![image](https://github.com/user-attachments/assets/9d01db01-e15e-4099-ad0d-178612cba175)

4. Formation of Gate
   * Deposit photo resist layer to define the areas to be protected, and then subsequently deposit mask-4. Then, UV light is applied, and the exposed area of photoresist is removed
   * Then, implantation of low energy boron at the surface of p-well using mask-4 to control the threshold occurs
   * Similarly, implantation of phosphorous/arsenic for n-well using mask-5 occurs
   * Fixing the oxide which is damaged by implantation steps by removing extra SiO2 using the hydroflouric acid and re-grow high quality SiO2 on p-substrate to contol the oxide thickness occurs next
   * Addition of polysilicon film subsequently occurs
   * Then, mask-6 is added and etching using photolithography occurs
   * Then, mask 6 is etched off to form the gate terminal

### Lightly Doped Drain [LDD] Formation

5. LDD Formation - the reason LDDs are created is to prevent the hot electron which can eventually cause Si - Si bonds break or create voltage that passes the 3.2eV barrier leading to issues with doped regions. The second major need is to prevent another effect, known as the short channel effect which can cause gate malfunctioning due to the drain field penetrating the channel.
   * Mask 7 and 8 are created for NMOS (lightly doped N-type) and PMOS (lightly doped P-type) respectively.
   * Heavily doped impurity (N+ for NMOS and P+ for PMOS) are added for the actual source and drain but the lightly doped impurity which are also added help maintain spacing between the source and drain and prevent hot electron effect and short channel effect.
   * To protect the lightly doped regions, we also add SiO2 and create spacers using plasma anisotropic etching

### Source and Drain Formation

6. Source and Drain Formation
   * Thin screen oxide is added to avoid channeling during. Channeling is when implantations dig too deep into substrate which is very problematic
   * We create Mask-9 is for N+ implantation and Mask-10 for P+ implantation
   * The side wall spacers maintain the N-/P- while implanting the N+/P+
   * High temperature annealing is done as well

### Local Interconnect Formation

7. Local Interconnect Formation - Steps to Form Connects and Interconnects [LOCAL] - these are very important as they help in controlling the electrical charecteristics. These are also the only things accessible to the end user.
   * The thin screen oxide is removed for opening up the source, drain and gate for contact building. We use Titanium as it has less resistance.
   *  Titanium Diselenide [Ti2Si2] is used for local interconnects.
   * Mask 11 is formed and Titanium Nitride [Ti N] is etched off by RCA cleaning to create the first level contact

### Higher Level Metal Formation

8. Higher Level Metal Formation
   * The previous steps in the MASK process have created an uneven surface layer. A layer of Silicon Dioxide [SiO2] doped with phosphorous or boron -[boron reduces the temperature] [known as phosphosilicate glass and borophosphosilicate glass] is deposited on the wafer surface.
   * Then, the surface is polished using the CMP [Chemical Mechanical Polishing] technique to planarize the surface.
   * Contact holes are created through photolithography.
   * Various masks are used for the various processes after this:-
      * Mask 12 is created for the first contact holes
      * Mask 13 is used for the first Aluminum contact layer, which the contact holes are connected to.
      * Mask 14 creates the second contact holes
      * Mask 15 is similarly, for the second Aluminum contact layer
      * Finally, we use Mask 16 for making contact to topmost layer

### Lab Introduction to SKY130 Basic Layers Layout and LEF using Inverter

![image](https://github.com/user-attachments/assets/37a5bcbf-3572-4ef9-914c-560dc5c33e37)

In sky130A, the first layer is the LDD or local-i and then the m1, m2 layers and so on. The power and ground lines are located in m1. When polysilicon crosses ndiffusion then NMOS and if polysilicon crosses pdiffusion then PMOS is created. The output of the layout is the LEF file, which is used by the router in APR to get the location of standard cell pins for proper routing. So it is essentially an abstract form of the layout of a standard cell.

### Lab Steps to Create STD Cell Layout and Extract SPICE Netlist

For the SPICE extraction of the custom inverter layout, we can enter the following commands in the TCKON -:
* extract all
* ext2spice cthresh 0 rthresh 0 --> This extracts the parasitic information
* ext2spice

then type the following commands

![image](https://github.com/user-attachments/assets/cc5582c1-4100-46ae-a90c-418187cebdc8)

![image](https://github.com/user-attachments/assets/ffe925a1-0d6a-4bee-8848-2f061c88955d)

## SKY130 Tech File Labs
### Lab Steps To Create Final SPICE deck using SKY130 tech

The default SPICE deck file using Sky130 is seen in the previous section. Now, we can modify the file to plot a transient response, which would then create a final SPICE deck file by editing as below.

![image](https://github.com/user-attachments/assets/a2e7840a-1423-4b0f-9efd-4490d0e11074)

![image](https://github.com/user-attachments/assets/139e2ab7-d218-4cbe-b13a-8ff47edbeed1)

To load the SPICE file for simulation in NGSPICE, type the following command : ngspice sky130A_inv.spice

### Lab Steps to Characterise Inverter using SKY130 Model Files

![image](https://github.com/user-attachments/assets/211c262e-702f-476b-957c-83c401146364)

![image](https://github.com/user-attachments/assets/f5185d39-8d45-495a-a93f-0f5fb979e74b)

edited:-

![image](https://github.com/user-attachments/assets/bb65ac6a-3268-4802-b077-a9baac347575)

![image](https://github.com/user-attachments/assets/47846601-73a0-4e94-8f2d-d299bf5f502a)

![image](https://github.com/user-attachments/assets/f34d4bd4-3b0b-4d88-9a7f-c1ce3fad72f3)

![image](https://github.com/user-attachments/assets/db1799d4-f9f0-41a9-b098-ddffd6dfb83f)

Using this we can charecterise 4 parameters:-



![image](https://github.com/user-attachments/assets/ad99eac2-8880-4d6d-b267-d8dbf86af9bd)
