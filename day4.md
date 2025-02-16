# DAY 4

## Timing Modelling Using Delay Tables

### Lab Steps To Convert Grid Information Into Track Information

sky130_inv.mag located in vsdstdcelldesign directory contains all information like PG and port information, logic etc. OpenLANE is a PnR tool and hence does not require the full information present in the .mag file.
The only information required is the boundary, power and ground rails, and the inputs & outputs information. This is why we use .lef files. Our next objective is to extract the LEF file from the MAGIC file to plug that into the picorv32a design. The guidelines to be followed while making a standard cell is as follows :-

1. The input and output ports must lie at the intersection of the horizontal and vertical tracks (this is to ensure the routes can reach the ports).

2. The width and height of the standard cell must be odd multiples of the track's horizontal and vertical pitch respectively

Tracks mean the horizontal and vertical metal layers on which routing can occur. The grid formed by the intersection of horizontal and vertical tracks creates a routing grid, also k/a a routing matrix.

The ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info contains track information. Before and After changing the grid values :-

![image](https://github.com/user-attachments/assets/ef331cc8-ec01-4be3-bd5a-d05945e3026e)

