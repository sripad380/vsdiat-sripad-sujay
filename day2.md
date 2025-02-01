# SKY130 Day 2: Good vs Bad Floorplan and Introduction to Library Cells
## Chip Floor Planning Considerations
### Utilisation Factor and Aspect Ratio

The first step in physical design is to **_define the width and height of the core and die_** : Beginning with a very simple netlist, that can extrapolated later we will first draw a basic diagram in the form of symbols that we will later convert into physical designs. We will take each cell and give it a standard dimensions. As an example here, each unit will be 1 unit x 1 unit - i.e. 1 sq. unit in size, and since there are 4 gates/flip-flops here, the total size of the silicon wafer will 4 sq. units.

![image](https://github.com/user-attachments/assets/8747c15b-ae81-4f3d-8d4b-735714675bf4)

All logical cells will be placed inside the core - which is part of the die. If the logical cells occupy the core fully, it is known as 100% utilisation. Utilisation factor = Area occupied by netlist / Total area of core. In this case, we see that utilisation factor is 100%, but practically it is usually 50%. Aspect Ratio is the ratio between height and width. If the chip is square - it is 1, else the chip is rectangular in shape.
![image](https://github.com/user-attachments/assets/037a4899-e7dc-4521-8f19-46a639f5ab40)
![image](https://github.com/user-attachments/assets/d2969a34-0da6-4d0b-a74d-8276eca217df)
