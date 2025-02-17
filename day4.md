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
