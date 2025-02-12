# Day 1
Inception of open-source EDA, OpenLANE and Sky130 PDK

To invoke openLANE flow, type docker

after that type ./flow.tcl -interactive

% package require openlane 0.9

% prep -design picorv32a

![lab 1 screenshot 1](https://github.com/user-attachments/assets/3b450bdf-2117-484b-9672-0cbb1b587db4)

following this run_synthesis
![lab 1 screenshot 2](https://github.com/user-attachments/assets/37577795-142a-464f-b1f3-73469ba84a38)


![lab 1 screenshot 3](https://github.com/user-attachments/assets/def3ec2f-478e-4bd5-97b3-18445273bf87)

Flop ratio= Number of D Flip flops/ Total number of cells

percentage of DFF's = flop ratio x 100

1613/14876

0.10842968539

10.84%

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Day 2
Good floorplan vs bad floorplan and introduction to library cells

invoke the OpenLANE flow using same steps in day 1 

After that perform floorplan using run_floorplan
![lab 2 screenshot 1](https://github.com/user-attachments/assets/952e4ce0-4e05-4cdc-9fbd-66a9c15a6df7)

Screenshot of contents of floorplan def
![lab 2 screenshot 2](https://github.com/user-attachments/assets/0196ef35-08e4-47c5-9229-79a024d001db)
1 micron = 1000 unit distance

Die width in unit distance == 660685 , Hence Die width in microns == 660.685

Die height in unit distance == 671405 , Hence Die height in microns == 671.405

Area = width x height = 660.685 x 671.405 = 443587.212425 microns



![lab 2 screenshot 4](https://github.com/user-attachments/assets/12028fd9-611b-457e-96ec-e4ff72c55729)

open floorplan in magic using:-  $ magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &


![lab 2 screenshot 3](https://github.com/user-attachments/assets/18b2b3ac-24c3-4e7a-9808-479297aa1d06)

run placement using run_placement and review the placement in magic

![lab 2 screenshot 5](https://github.com/user-attachments/assets/127e2306-5348-429d-9570-9cffec85c11b)
![lab 2 screenshot 6](https://github.com/user-attachments/assets/fb465419-f984-43a3-a616-87f9c20920f5)
![lab 2 screenshot 7 placement screenshot zoomed](https://github.com/user-attachments/assets/991c2be3-8f2d-43e3-9ce3-4f3c1b40962a)



------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Day 3
Design library cell using Magic Layout and ngspice characterization


Clone custom inverter standard cell design from github repository
![lab 3 screenshot 1](https://github.com/user-attachments/assets/c6950da8-ff64-4735-adc4-d8db5936b6a4)
![lab 3 screenshot 2](https://github.com/user-attachments/assets/4b1d28ba-18e4-4a16-b73a-666f0c2995bf)

Spice extraction of inverter in magic

use the following commands

extract all

ext2spice cthresh 0 rthresh 0

ext2spice

![lab 3 screenshot 3](https://github.com/user-attachments/assets/8fe68f1c-1d7e-4f47-9563-d49ccb0d3300)

![lab 3 screenshot 4](https://github.com/user-attachments/assets/73746e9f-5028-4232-a585-f064d42fae55)
Update values for ngspice

![lab 3 screenshot 6](https://github.com/user-attachments/assets/8c37a999-eb21-476f-bebd-4d52a7ee7bb7)

Command to load spice file for simulation to ngspice

ngspice sky130_inv.spice

plot y vs time a

![lab 3 screenshot 8](https://github.com/user-attachments/assets/2c8868e5-e552-4bb7-9412-8c897e7393d6)

Here we will calculate Rise time delay, Fall time delay and Propogation delay.
![lab 3 screenshot 7](https://github.com/user-attachments/assets/536e2525-3f47-440a-aa81-b473307974ff)

![lab 3 screenshot 9](https://github.com/user-attachments/assets/14c0c19e-6ebf-4e99-92e3-77a321ea2818)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Day 4
Pre-layout timing analysis and importance of good clock tree

![lab 4 ss 1](https://github.com/user-attachments/assets/9b590a3d-b923-497f-8e23-14ea709130a5)

Screenshot of tracks.info of sky130_fd_sc_hd

![lab 4 ss 2](https://github.com/user-attachments/assets/d95d936a-ad36-4bec-be32-b6652b3865a9)

Get syntax for grid command: - 
help grid

Set grid values accordingly : - 
grid 0.46um 0.34um 0.23um 0.17um

![lab 4 ss 3](https://github.com/user-attachments/assets/f61c9788-036a-4eca-9bee-ac0b588e3ca1)

Here we define ports. Ports are required while we want to extract the lef file. When we extract the lef file these ports are defined as pins
![lab 4 ss 4](https://github.com/user-attachments/assets/e6f124c5-c277-4e22-930d-e6d04a59d028)

Check for the file in vsdstdcelldesign folder

![lab 4 ss 5 updated ](https://github.com/user-attachments/assets/6de750d5-9f6a-4fe5-80d8-5d8aa4d94c5a)

Create the LEF file
![lab 4 ss 6](https://github.com/user-attachments/assets/2149a818-85a3-4de1-9962-c9b12c1703a8)

inspect the newly created sky130_inv.lef file 
![lab 4 ss 7 lef file](https://github.com/user-attachments/assets/3102cf12-8385-49c3-8b3c-24bdfad583f5)

Edit the config.tcl file to include new LEF and LIB files
![lab 4 ss 8](https://github.com/user-attachments/assets/85f657cf-a60c-44c1-bfed-6fa8f288648a)

Start OpenLANE and prepare the customised design for synthesis. This time during preparation, current file should be overwritten

In my case the prep command was prep -design  picorv32a -tag 29-01_20-58 -overwrite 

This error will occur. Change LIB_MIN to LIB_FASTEST and LIB_MAX to LIB_SLOWEST
![lab 4 ss 9 funny error 1](https://github.com/user-attachments/assets/f4f46df7-012c-40b6-9bcb-19395426f280)

run_synthesis
![lab 4 ss 10](https://github.com/user-attachments/assets/79eaa72f-1a13-4d87-90a5-f13f0d3873b9)
![lab 4 ss 11](https://github.com/user-attachments/assets/6aab928e-a859-42c7-99e2-539679239300)

run the following commands to run floorplan as regular method returns error

init_floorplan

place_io

tap_decap_or

Open the floorplan in magic

![lab 4 ss 14](https://github.com/user-attachments/assets/e79a910d-2fb7-4fa5-bee7-a78440fdc3cf)


run_placement and then open in magic
![lab 4 ss 13](https://github.com/user-attachments/assets/0f2200e7-004d-482a-82df-55f445e5741d)

![lab 4 ss 15](https://github.com/user-attachments/assets/1de632a0-687c-4aec-80b1-a0cbaddb8feb)

run the clock tree analysis in openlane using run_cts
![lab 4 ss 16](https://github.com/user-attachments/assets/40889373-c737-492a-b39f-e453d135d5a8)

Post CTS Analysis

![lab 4 ss 17](https://github.com/user-attachments/assets/cd25d5bc-5dc5-455e-83fa-d4d1be388da4)
![lab 4 ss 18](https://github.com/user-attachments/assets/d4730a6f-11a8-4373-9eda-31c1b21ec839)

Layout after CTS

![lab 4 ss 19](https://github.com/user-attachments/assets/c908cd03-7afc-453b-8da8-58d790da73fd)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Day 5
Final steps for RTL2GDS using tritonRoute and openSTA

Generate PDN (Power Delivery Network) 

![lab 5 ss 1](https://github.com/user-attachments/assets/5792c381-e246-42c8-addd-4eff8b96ebb4)
![lab 5 ss 2](https://github.com/user-attachments/assets/45a8f801-6281-45e0-b559-5a6c4dbc0c2b)

open in magic tool

![lab 5 ss 3](https://github.com/user-attachments/assets/22498392-ede2-4569-884d-b9e87cdda4f5)

run_routing

![lab 5 ss 5](https://github.com/user-attachments/assets/b9bb09a1-a09d-42ae-8d17-56175f131ba6)
![lab 5 ss 4](https://github.com/user-attachments/assets/c027eb8e-4a84-407d-b831-5a814e57882e)

There are no drc and timing violations

load routed def in magic in another terminal
![lab 5 ss 6 routed def](https://github.com/user-attachments/assets/4c8ac608-f2ce-4970-a9d9-4fabed96937f)
![lab 5 ss 7](https://github.com/user-attachments/assets/6ac9ebee-cca8-4f8e-9324-878eb0e1bcd0)

Run Static Time Analysis

![lab 5 ss 8](https://github.com/user-attachments/assets/d3d13f99-f95e-4ae9-9fae-c2df28a23294)
![lab 5 ss 9](https://github.com/user-attachments/assets/362749ef-31c7-412c-b7ce-b44d74b0b0cf)

Final Layout

![lab 5 ss 10](https://github.com/user-attachments/assets/de94e93c-d70d-4a9a-9d6a-eafb698c9ea1)





