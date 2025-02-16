# SKY130 Day 3: Design Library Cell Using Magic Layout and NGSPICE Characterization

## Labs for CMOS Inverter NGSPICE Simulations

### IO Placer Revision

OpenLANE configurations can be modified within the shell environment. The IO Mode is typically set to "random equidistant" [env(FP_IO_MODE)_1]. However, this can be altered with the following command, executed after the floorplan stage: `set ::env(FP_IO_MODE) 2`. This change will configure the IO input/output pins to be non-equidistant in mode 2 (as opposed to the default mode 1).

Subsequently, the floorplan can be re-run to ensure that the pins are now placed according to the Hungarian algorithm, stacked one over the other.

![image](https://github.com/user-attachments/assets/9c7156db-e377-45cf-af1e-adc8eee8b4d5)

### SPICE Deck Creation for CMOS Inverter

The SPICE deck comprises connectivity information about netlists, input configurations, TAPs for outputs, etc. Typically, the component values are:

- PMOS and NMOS: 0.375 µm / 0.25 µm (i.e., the channel length is 0.25 micron and the channel width is 0.375 micron).

![image](https://github.com/user-attachments/assets/4b4afc64-1a45-4226-830c-040a4b7eff52)

### SPICE Simulation Lab for CMOS Inverter

The syntax for the SPICE deck netlist for PMOS and NMOS is as follows: [component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]. Note that all components in a netlist are described based on their nodes and values.

![image](https://github.com/user-attachments/assets/361f7be0-ab29-42f8-b316-c9cf0f7c50e5)

First line:
- M1 -> component name
- out -> output
- in -> input
- vdd -> source
- vdd -> substrate
- PMOS -> transistor type
- W=0.375u -> width
- L=0.25u -> length

Second line:
- M2 -> component name
- out -> output
- in -> input
- 0 -> source
- 0 -> substrate
- NMOS -> transistor type
- W=0.375u -> width
- L=0.25u -> length

![image](https://github.com/user-attachments/assets/0c433b21-0422-42bf-8139-ba11393ca1e9)

The green line represents the output for opposing input.

![image](https://github.com/user-attachments/assets/880af982-5994-43eb-9390-0d8a392c1cbc)

### Switching Threshold Vm

![image](https://github.com/user-attachments/assets/492c7281-14a2-48d1-bf21-68e32aa83350)

Note:
- The shapes of the graphs are almost identical, indicating that CMOS is a robust device.
- The parameters determining the robustness of the CMOS are the switching threshold and the propagation delay.
- The switching threshold is the point where the input voltage equals the output voltage.
- Vm is the point where Vin = Vout.

### Static and Dynamic Simulation of CMOS Inverter

To determine Vm, a DC transfer analysis is used. The simulation involves sweeping from 0V to 2.5V in 0.05V steps.

![image](https://github.com/user-attachments/assets/8fc3ef47-fdad-4056-bf4a-91ba1f1630e3)

To determine the propagation delay, a transient analysis is performed when a pulse is applied to the CMOS.

![image](https://github.com/user-attachments/assets/2af5f996-0722-4351-bc43-1e64dbcb1625)

![image](https://github.com/user-attachments/assets/37375b44-2fcb-4f5d-adc0-8361f70d170a)

### Lab Steps to GitClone VSDSTD Cell Design

A GitHub repository containing the inverter files is available at [this link](https://github.com/nickson-jose/vsdstdcelldesign). The steps to clone and observe the layout are as follows:

1. Clone the repository with the custom inverter design using the command: `git clone https://github.com/nickson-jose/vsdstdcelldesign`
2. Copy the tech file to the vsdstdcelldesign directory with the command: `cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/`
3. Open the custom inverter layout in MAGIC with the command: `magic -T sky130A.tech sky130_inv.mag &`

![image](https://github.com/user-attachments/assets/e56ec8bd-0640-4058-956f-ce998f0375f5)

## Inception of Layout and CMOS Fabrication Process

### Create Active Regions

1. Select a substrate where the entire design is fabricated. The most common substrate is a P-doped silicon substrate, which is ideally less doped than its wells.
2. Create an active region for transistors. Isolation between the pockets is necessary, which can be achieved through the following steps:
   - Grow 40 nm of silicon dioxide.
   - Deposit 80 nm of silicon nitride.
   - Deposit a layer of photoresist.
   - Deposit mask-1 layer on top of the photoresist, covering the areas that must not be etched away (protects the two transistor active regions).
   - Apply UV light to remove the layers on the unmasked regions.
   - Remove mask-1 and photoresist layers.
   - Place the chip in the furnace to grow the oxide in other areas.
   - Remove the Si3N4 layer using hot phosphoric acid to leave only the p-substrate and SiO2.

### Formation of N and P Well

3. Form the P well and N well:
   - Deposit a photoresist layer and define the areas to protect by depositing mask-2 and mask-3. Mask-2 protects the N-Well (PMOS side) while the P-Well (NMOS side) is being fabricated, and mask-3 protects the P-Well while the N-Well is being formed.
   - Apply UV light to remove the exposed photoresist.
   - Place the chip in the furnace to diffuse boron and phosphorus to form the wells. This process is called the Twintub process.

### Formation of Gate Terminal

The gate terminal is where the threshold voltage is controlled, as illustrated below:

![image](https://github.com/user-attachments/assets/9d01db01-e15e-4099-ad0d-178612cba175)

4. Form the gate:
   - Deposit a photoresist layer to define the areas to be protected, then subsequently deposit mask-4. Apply UV light, and remove the exposed photoresist.
   - Implant low-energy boron at the surface of the p-well using mask-4 to control the threshold.
   - Similarly, implant phosphorus/arsenic for the n-well using mask-5.
   - Fix the oxide damaged by implantation steps by removing extra SiO2 using hydrofluoric acid and re-grow high-quality SiO2 on the p-substrate to control the oxide thickness.
   - Add a polysilicon film subsequently.
   - Add mask-6 and perform etching using photolithography.
   - Etch off mask-6 to form the gate terminal.

### Lightly Doped Drain (LDD) Formation

5. Form the LDD to prevent the hot electron effect, which can eventually cause Si-Si bonds to break or create voltage that surpasses the 3.2 eV barrier, leading to issues with doped regions. Another major need is to prevent the short channel effect, which can cause gate malfunctioning due to the drain field penetrating the channel.
   - Create mask-7 and mask-8 for NMOS (lightly doped N-type) and PMOS (lightly doped P-type), respectively.
   - Add heavily doped impurity (N+ for NMOS and P+ for PMOS) for the actual source and drain. The lightly doped impurity added helps maintain spacing between the source and drain and prevents hot electron effect and short channel effect.
   - Protect the lightly doped regions by adding SiO2 and creating spacers using plasma anisotropic etching.

### Source and Drain Formation

6. Form the source and drain:
   - Add a thin screen oxide to avoid channeling during implantation. Channeling occurs when implantations dig too deep into the substrate, which is problematic.
   - Create mask-9 for N+ implantation and mask-10 for P+ implantation.
   - The sidewall spacers maintain the N-/P- while implanting the N+/P+.
   - Perform high-temperature annealing.

### Local Interconnect Formation

7. Form local interconnects, which are crucial for controlling the electrical characteristics. These are the only components accessible to the end user.
   - Remove the thin screen oxide to open up the source, drain, and gate for contact building. Use titanium due to its low resistance.
   - Use titanium disilicide (TiSi2) for local interconnects.
   - Form mask-11 and etch off titanium nitride (TiN) by RCA cleaning to create the first level contact.

### Higher Level Metal Formation

8. Form higher-level metal layers:
   - The previous steps in the MASK process create an uneven surface layer. Deposit a layer of silicon dioxide (SiO2) doped with phosphorus or boron (boron reduces the temperature), known as phosphosilicate glass and borophosphosilicate glass, on the wafer surface.
   - Polish the surface using the chemical mechanical polishing (CMP) technique to planarize the surface.
   - Create contact holes through photolithography.
   - Use various masks for different processes:
      - Mask-12 is created for the first contact holes.
      - Mask-13 is used for the first aluminum contact layer, which the contact holes connect to.
      - Mask-14 is created for the second contact holes.
      - Mask-15 is used for the second aluminum contact layer.
      - Finally, mask-16 is used for making contact to the topmost layer.

### Lab Introduction to SKY130 Basic Layers Layout and LEF Using Inverter

![image](https://github.com/user-attachments/assets/37a5bcbf-3572-4ef9-914c-560dc5c33e37)

In SKY130A, the first layer is the LDD or local interconnect, followed by the m1, m2 layers, and so forth. The power and ground lines are located in m1. When polysilicon crosses n-diffusion, NMOS is created, and when polysilicon crosses p-diffusion, PMOS is created. The output of the layout is the LEF file, used by the router in APR to get the location of standard cell pins for proper routing. It is essentially an abstract form of the layout of a standard cell.

### Lab Steps to Create Standard Cell Layout and Extract SPICE Netlist

For the SPICE extraction of the custom inverter layout, enter the following commands in the TCKON:

- `extract all`
- `ext2spice cthresh 0 rthresh 0` --> This extracts the parasitic information.
- `ext2spice`

Then, type the following commands:

![image](https://github.com/user-attachments/assets/cc5582c1-4100-46ae-a90c-418187cebdc8)

![image](https://github.com/user-attachments/assets/ffe925a1-0d6a-4bee-8848-2f061c88955d)

## SKY130 Tech File Labs

### Lab Steps to Create Final SPICE Deck Using SKY130 Tech

The default SPICE deck file using SKY130 is seen in the previous section. Modify the file to plot a transient response, creating a final SPICE deck file by editing as shown below.

![image](https://github.com/user-attachments/assets/a2e7840a-1423-4b0f-9efd-4490d0e11074)

![image](https://github.com/user-attachments/assets/139e2ab7-d218-4cbe-b13a-8ff47edbeed1)

To load the SPICE file for simulation in NGSPICE, type the following command: `ngspice sky130A_inv.spice`.

### Lab Steps to Characterize Inverter Using SKY130 Model Files

![image](https://github.com/user-attachments/assets/211c262e-702f-476b-957c-83c401146364)

![image](https://github.com/user-attachments/assets/f5185d39-8d45-495a-a93f-0f5fb979e74b)

Edited:

![image](https://github.com/user-attachments/assets/bb65ac6a-3268-4802-b077-a9baac347575)

![image](https://github.com/user-attachments/assets/47846601-73a0-4e94-8f2d-d299bf5f502a)

![image](https://github.com/user-attachments/assets/f34d4bd4-3b0b-4d88-9a7f-c1ce3fad72f3)

![image](https://github.com/user-attachments/assets/db1799d4-f9f0-41a9-b098-ddffd6dfb83f)

![image](https://github.com/user-attachments/assets/adb24d98-de13-4ccb-976c-a04cc3941122)

![image](https://github.com/user-attachments/assets/38a17a03-7c64-436a-ad98-6d910438c93d)

### Lab Introduction to Magic Tool Options and DRC Rules

An introduction to Magic's [design rule check](http://opencircuitdesign.com/magic/tutorials/tut6.html).

### Lab Introduction to SKY130 PDKs and Steps to Download Labs

To download the lab files, use the following command:

`wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz`

![image](https://github.com/user-attachments/assets/fb769f81-d6e6-42f4-91ab-9b306faf7adc)

Extract these files using:

`tar xfz drc_tests.tgz`

![image](https://github.com/user-attachments/assets/0aa6eb3a-9e97-496b-bc38-3046fdedfcb9)

Navigate to the drc_tests directory using:

`cd drc_tests`

Then open Magic using:

`magic -T`

> Note: Sir used `magic -d XR`, but I find using `magic -T` to be fine.

### Lab Introduction to Magic and Steps to Load SKY130 Tech Rules

In the empty Magic prompt, click "File" (located in the top left corner) and select "met3.mag" to open it, displaying the following:

![image](https://github.com/user-attachments/assets/071f6a28-a25d-4b78-b07a-bb6f58d74813)

The broken rules can be found [here](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3).

The broken rules list is as follows:
- m3.1 -> Width should be greater than 0.3 µm.
- m3.2 -> Distance between two metals should be greater than 0.3 µm.
- m3.3c -> Minimum distance of features attached/extending from huge_met 3 for a distance of up to 0.4 µm to metal 3.
- m3.3d -> Open minimum spacing of huge_met3 to metal 3, excluding features checked by M3.3a.
- m3.5 -> Via 2 must be influenced by metal 3 or one of the 2 adjacent sides by at least some value.
- m3.6 -> Minimum area of metal 3 must be 0.24 µm².

### Lab Exercise to Fix poly.9 Error in SKY130 Tech-File

Type `load poly` in the tkcon window:

![image](https://github.com/user-attachments/assets/a862d281-7803-432f-bd21-faf1f7d86933)

As stated [here](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#poly), the poly resistor spacing to poly (no overlap) to diff/tap should be at least 0.48 µm. The gridlines here are 0.1 µm lengthwise and breadthwise. It is clear that the spacing is less than 0.48 µm:

![image](https://github.com/user-attachments/assets/ea5243b0-34da-4637-bab8-4796f93645f5)

Open the sky130A.tech file from the drc_tests directory. The rules included for poly.9 are only for the spacing between the n-poly and p-poly resistor with diffusion. We will now add new rules for the spacing between the poly resistor and poly non-resistor. The two new rules are highlighted below:

![image](https://github.com/user-attachments/assets/9b46c3f5-f921-4c36-8bc8-795a8eb1114f)

![image](https://github.com/user-attachments/assets/4a05e5b3-1761-4f1a-b132-4e7cb533718b)

Then, enter the commands as shown:

![image](https://github.com/user-attachments/assets/62f9fa3b-df79-459b-b071-a3430a6b0028)
