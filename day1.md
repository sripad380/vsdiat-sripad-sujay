# Day 1

## How To Talk To Computers

### Introduction to QFN-48 Package, Pads, Chip, Core, Die, and IPs

A commonly used Arduino board, the Arduino Leonardo, is shown below. It uses a chip that is depicted from a high-level view.

![arduino leonardo](https://github.com/user-attachments/assets/f2983e10-4bd6-48bf-a196-1a20efab5d50)

This can be represented via a block diagram as shown below:

![image](https://github.com/user-attachments/assets/11eb7d67-ef18-40ae-b0e6-0f4aaff15cc2)

The picture represents a large block called SoC. This is commonly called a chip, but in reality, it is a package inside which the chip is present. One type of package commonly used in Arduino boards is the QFN-48 package.

![image](https://github.com/user-attachments/assets/3f481c5d-a0b6-48e0-bbf3-3e92df517f9f)

A chip is usually connected to many pins or I/O (inputs or outputs) surrounding it. The position and placement of these pins depend on their PCB (Printed Circuit Board). The components include:
- Pads
- Die
- Core
  - Macro
  - IPs

A schematic of all components is shown below:

![image](https://github.com/user-attachments/assets/8efb5383-ef23-4209-9c6b-66a3d97e0b8c)

### Introduction to RISC-V

RISC-V is the language of the computer. When a program is to be run on hardware, it is first compiled into an assembly language program like RISC-V. It is then converted to machine language by an assembler.

![image](https://github.com/user-attachments/assets/2f25247e-e849-4f9a-92c1-39fe8946bfb2)

### From Software Applications to Hardware

Applications run on hardware such as laptops, tablets, etc., powered by chips. However, apps are coded in complex programs that require conversion into binary code for execution. This is done using:
- Operating systems (OS)
- Compilers
- Assemblers

## SoC Design and Openlane

### Introduction to All Components of Open Source Digital ASIC

Open source digital ASIC requires mainly three things:
- RTL IPs
- PDK Data
- EDA Tools

Until June of 2020, there was no available Open Source PDK. Then Google partnered with Skywater to release a 130nm open source PDK.

![image](https://github.com/user-attachments/assets/d0af3ac1-25f2-4826-87bb-e66cf12d7bfd)

### Simplified RTL to GDS Flow

RTL to GDSII (Register Transfer Level to Graphic Design System II) has many steps:
- Synthesis: Converts hardware description languages such as VERILOG into gate-level representations.
- Floor Planning: Involves optimizing chip performance, area utilization, and connectivity.
  - Chip Floor Planning: Planning all the components of a chip.
  - Macro Floor Planning: Planning all the components in a macro.
  - Power Planning: Planning all power rails.
- Placement: Placing components efficiently. It involves:
  - Global Placement: Placing in the best positions without following any rules.
  - Detailed Placement: Adjusting the global placement to follow all rules.
- Creating a Clock Distribution Network: Ensuring proper timing with minimal differences.
- Routing: Connecting components efficiently. It involves:
  - Global Routing: Connecting in the best ways without following any rules.
  - Detailed Routing: Adjusting the global routing to follow all rules.
- Signoff: Verification through:
  - Design Rule Check (DRC): Ensures the design complies with manufacturing guidelines.
  - Layout vs. Schematic (LVS): Ensures the layout matches the schematic.
  - Static Timing Analysis (STA): Evaluates timing behavior of the digital circuit.

### Introduction to OpenLANE and Strive Chipsets

OpenLANE is an open-source digital ASIC jointly developed by efabless and Google, designed to automate the entire design process flow based on several open-source components like OpenROAD, SPEF-Extractor, etc.

![image](https://github.com/user-attachments/assets/0863a336-b2da-4dee-8b50-31ec56111171)

![image](https://github.com/user-attachments/assets/1c49062f-62e2-461f-9e58-2fa470ae53d4)

### Introduction to OpenLANE Detailed ASIC Design Flow

![image](https://github.com/user-attachments/assets/d984fc07-baff-4cdf-9f23-472dec3cb488)

- Synthesis Exploration: Generates a delay x area report.
  ![image](https://github.com/user-attachments/assets/acd66af2-a2bd-43c9-b948-a69f12e0d8de)
- Design Exploration: Changes design configuration to find the best configuration.
  ![image](https://github.com/user-attachments/assets/180e522f-bce7-4657-98d9-0037496d254f)
- OpenLANE Regression Testing:
  ![image](https://github.com/user-attachments/assets/749d7dd4-d529-43cf-a6d6-a575b5ca9269)
- Design for Test (DFT):
  ![image](https://github.com/user-attachments/assets/4f12aa0b-bd1f-4877-9b91-799e4bd05570)
- Physical Verification (DRC & LVS): Uses Magic for DRC and Magic and Netgen for LVS.
- Logic Equivalence Check (LEC): Ensures the physical implementation and the netlist have the same logic.
- Dealing with Antenna Rules Violations:
  ![image](https://github.com/user-attachments/assets/3da9f1a8-bb3c-4d3a-9b9a-7a25a9d0926b)
  ![image](https://github.com/user-attachments/assets/d1d068b6-7975-4c0a-a4f2-a3ac78ceed43)
  ![image](https://github.com/user-attachments/assets/42c53bbf-763e-426f-9162-d6362a4a73b8)
- Timing Analysis (STA): Includes a synthesized netlist along with other data.

## Get Familiar with Open Source EDA Tools

### OpenLANE Directory Structure in Detail

OpenLANE is more of a flow than a tool, automating the entire RTL to GDSII flow to make it clean and open source. It is similar to commercial EDA tools.

Exploring Open Source directory through Linux terminal steps:
1. Open the virtual machine and then open the terminal on it.
![image](https://github.com/user-attachments/assets/bf449a46-03f7-4698-8c42-76f1ac0636d0)

2. Type `cd Desktop/work/tools`.
![image](https://github.com/user-attachments/assets/01a91b5a-f681-4ab8-8373-80b8f95e5b5e)

3. Type `ls -ltr` to see everything inside tools. `ltr` defines the order of the items.
![image](https://github.com/user-attachments/assets/ac20b708-1950-48f7-8086-f7dc0041b7b3)

4. Change directory to `openlane_working_dir` by typing `cd openlane_working_dir`. List the contents using `ls -ltr`.
5. Explore both `pdks` and `openlane` directories in sequence.
6. Type `cd pdks` and then `ls -ltr` to view the contents in pdks. Explore `sky130A` similarly.
7. `libs.tech` contains all tool-specific files.
![image](https://github.com/user-attachments/assets/6d110b67-7917-4791-b970-5d39c56fd65c)

8. Opening the `magic` directory in `libs.tech`:
![image](https://github.com/user-attachments/assets/dd5f79ff-8ee9-4eaf-8e79-b70baef7bbe7)

9. `libs.ref` contains all the technology/foundry related processes. Explore `sky130_fd_sc_hd`.
![image](https://github.com/user-attachments/assets/dbccc004-a5f5-4f88-a128-6244da3b01dc)

10. Next, open `openlane`:
![image](https://github.com/user-attachments/assets/10d2ef9d-f278-41a6-8f71-c495e3a25f37)

### Design Preparation Step

To open OpenLANE, use the docker command. Then type `./flow.tcl -interactive`. The icon changes to a % symbol. Then type: `package require openlane 0.9`.

![image](https://github.com/user-attachments/assets/af750af7-de01-4d1c-a04f-364d2643fe94)

To setup the design of OpenLANE, prepare the design using the command `prep -design picorv32a`:

![image](https://github.com/user-attachments/assets/b20a049c-8044-46c7-ae0e-e7e1fbbfbc1f)

The command sets up a filesystem where OpenLANE can store results. This creates a folder inside the `picorv32a` directory containing command log files, results, and the related data.

### Review Files after Design Prep and Run Synthesis

Go to `tmp` and type `less merged.lef` to view information on `picorv32a`:

![image](https://github.com/user-attachments/assets/c5d87e4f-bf41-4328-beb3-74737780fdff)

The `config.tcl` file contains specific values for many things in any design. Go to the main folder (runs/date&time) and type `less config.tcl`:

![image](https://github.com/user-attachments/assets/d5485085-4d55-4bd1-883c-4542c92972a4)

Type `run_synthesis` to run the synthesis:

![image](https://github.com/user-attachments/assets/d2e81cb2-5ea7-4438-89a1-e0bac12fb777)
