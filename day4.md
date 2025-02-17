# DAY 4

## Timing Modelling Using Delay Tables

### Lab Steps To Convert Grid Information Into Track Information

sky130_inv.mag located in vsdstdcelldesign directory contains all information like PG and port information, logic etc. OpenLANE is a PnR tool and hence does not require the full information present in the .mag file.
The only information required is the boundary, power and ground rails, and the inputs & outputs information. This is why we use .lef files. Our next objective is to extract the LEF file from the MAGIC file to plug that into the picorv32a design. The guidelines to be followed while making a standard cell is as follows :-

1. The input and output ports must lie at the intersection of the horizontal and vertical tracks (this is to ensure the routes can reach the ports).

2. The width and height of the standard cell must be odd multiples of the track's horizontal and vertical pitch respectively

Tracks mean the horizontal and vertical metal layers on which routing can occur. The grid formed by the intersection of horizontal and vertical tracks creates a routing grid, also k/a a routing matrix.

The ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info contains track information :-

![image](https://github.com/user-attachments/assets/9c902d4d-02d2-40cf-aec1-ba96c586676e)

so:

![image](https://github.com/user-attachments/assets/e88a6090-3405-4c35-bd53-5126c9a5dd8b)


![image](https://github.com/user-attachments/assets/8cd30bc0-d748-4876-a2b6-0eec513e4764)

we can satisfy the first condition (the pins lie at the intersection):-

![image](https://github.com/user-attachments/assets/2e41938f-050b-4576-9988-da4250085fcf)

we can satisfy the second condition :-

![image](https://github.com/user-attachments/assets/7b7c1a9d-018c-4af0-adab-1975fd314488)

### Lab Steps to Convert Magic Layout to STD Cell LEF

LEF [Library Exchange Format] which contains information of the standard cell library used in the design. They contain detailed information about the geometric shapes, sizes, layers, and other physical properties of individual cells or macros within the library. The instructions to set the port definitions are available [here](https://github.com/nickson-jose/vsdstdcelldesign#create-port-definition). Next, save the .mag file with a new filename by typing lef write in the tkcon terminal, which will generate a new lef file with the new filename :-

![image](https://github.com/user-attachments/assets/2c7c4a2c-5b02-4237-a231-9c678c2aabaf)
![image](https://github.com/user-attachments/assets/083ebe60-352a-4670-9f03-70711934db92)

### Introduction of Timing libs and Steps To Include New Cell in Synthesis

Inside this directory [pdks/sky130A/libs.ref/sky130_fd_sc_hd/lib/] are the liberty timing files for SKY130 PDK which have the timing and power parameters for each cell needed in STA. It can either be slow, typical, fast with different supply voltages (1v80, 1v65, 1v95), which are called PVT corners. The library named sky130_fd_sc_hd__ss_025C_1v80 describes the PVT corner as slow-slow [ie. delay is maximum], 25° Celsius temperature, at 1.8V power supply. Timing and power parameter of a cell are obtained by simulating the cell in a variety of operating conditions [i.e. different corners] and this data is represented in the liberty file, which characterizes all cells and is used during ABC mapping during synthesis stage which maps the generic cells to the actual standard cells available in the liberty file.

* Firstly, copy the extracted lef file - named sky130_vsdinv.lef along with its libraries using the commands

![image](https://github.com/user-attachments/assets/2d5c8235-3de2-489d-97fb-9dee58c018c0)

<img src="https://github.com/user-attachments/assets/57be4779-be99-493c-bd9c-3642280027e9" alt="image" width="600" height="9">

![image](https://github.com/user-attachments/assets/58c21642-5fa1-45fc-ae5a-b38f3af968b8)

and then you will get this ;-

![image](https://github.com/user-attachments/assets/399d21f1-c2e8-4f8d-8b4f-095d20a5e4b4)

* Subsequently, change the config.tcl file from this :-

![image](https://github.com/user-attachments/assets/0b4e4431-f79c-41fa-8f39-f09ef488eae5)

* to this :-


* After this, invoke the docker command and prepare the picorv32a design. Plug the new lef file to the OpenLANE flow through the following commands

```
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -15-0
set lefs [glob picorv32a/src/sky130_inv.lef]
add_lefs -src $lefs
```

this will give this as the result :-

