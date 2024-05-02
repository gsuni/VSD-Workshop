
Day-1 Inception of open source,EDA Openlane and Sky130 PDK

Design prearation steps

![Screenshot (157)](https://github.com/gsuni/VSD-Workshop/assets/99734954/3f129397-8035-4e0d-a4a2-7fedaf35c43c)

##Using following command to invoke openlane:<br>
```bash
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
```

![Screenshot (158)](https://github.com/gsuni/VSD-Workshop/assets/99734954/eb83edda-7f7e-4c43-86ac-e49be8be3847)

![Screenshot (159)](https://github.com/gsuni/VSD-Workshop/assets/99734954/1d7dafbe-dc84-4911-ba56-2bf59ac288f7)

### Synthesis
   
 To run the Synthesis
   
    ```bash
       run_synthesis
    ```
![Screenshot (160)](https://github.com/gsuni/VSD-Workshop/assets/99734954/de46dc11-5e5a-4438-a84d-02ca58fac170)


![Screenshot (161)](https://github.com/gsuni/VSD-Workshop/assets/99734954/44979009-ecea-4b7e-bc88-911e187fa7c4)

to check wheather the synthesis is donr or not:

```bash
openlane/design/picorv32a/runs/ls -lts/date/results
```

![Screenshot (162)](https://github.com/gsuni/VSD-Workshop/assets/99734954/5dca5a03-b0ba-4247-94be-ee00322f56d9)


#To calculate the  flip-flop ratio

![Screenshot (163)](https://github.com/gsuni/VSD-Workshop/assets/99734954/5c745f0c-7b3d-497c-b1fa-6f6287167556)


![Screenshot (164)](https://github.com/gsuni/VSD-Workshop/assets/99734954/bedf7494-f007-43e1-9e20-928a9239d23a)


   
       Flop Ratio = No. of D FlipfLops/Total No. of cells
      
                   =1613/14876
      
                   =0.10842
      
        Percentage of D FF's = 10.842%
        

 ## Day2 Good floorplan vs bad floorplan and introduction to library cells
   
 ## Floorplanning and Intro to Library Cells
 
```bash
  run_floorplan
```

![Screenshot (166)](https://github.com/gsuni/VSD-Workshop/assets/99734954/c5952dd2-3796-4b72-8454-5a57751f46bf)


![Screenshot (168)](https://github.com/gsuni/VSD-Workshop/assets/99734954/6b314092-168d-42c8-b85b-dc3becb4cdc1)

if you encounter error while executing the floorplanning 

![Screenshot (165)](https://github.com/gsuni/VSD-Workshop/assets/99734954/b76a209f-759a-47d1-831f-00530647dc5b)

- Press 'Esc' to enter command mode.
- While in command mode press 'i' to enter edit mode and then type the code which is given below.
- After editing is done press 'Esc' to once again enter command mode.
- Now type ':wq' and press enter to save and exit from file.

```bash
set ::env(CLOCK_NET) $::env(CLOCK_PORT)
set ::env(FP_CORE_UTIL) 65
set ::env(FP_IO_VMETAL) 4
set ::env(FP_IO_HMETAL) 3
```

To see the actual layout after the flow, we have to open the magic file by adding the command

```bash
magic -T /home/vsdsquadron/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef 
  read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![Screenshot (173)](https://github.com/gsuni/VSD-Workshop/assets/99734954/7ab28271-ffc4-4cf2-b261-a9e1f4072ebf)


![Screenshot (172)](https://github.com/gsuni/VSD-Workshop/assets/99734954/dbc4db5e-74e0-4641-a248-08368f4a1ae0)

![Screenshot (174)](https://github.com/gsuni/VSD-Workshop/assets/99734954/cd66b1f1-7176-444a-9884-203665ed400e)

Horizontal metal layer

![Screenshot (177)](https://github.com/gsuni/VSD-Workshop/assets/99734954/8dccb0b9-86f2-42e3-9164-97eeb8e0eede)

Vertical metal layer

![Screenshot (176)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d9358163-e49f-4b6a-9384-4ee83c121067)

To run the Placement, the command is
```bash
run_placement
```

![Screenshot (178)](https://github.com/gsuni/VSD-Workshop/assets/99734954/14ed8aab-b2e0-46fb-9f54-2b344609ba9e)



The Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below.  
  Commands to load placement def in magic
  
  ```bash
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![13](https://github.com/gsuni/VSD-Workshop/assets/99734954/91a6f682-352c-4e92-b096-3d1d32cff7cc)




DAY 3: Design library cell using Magic Layout and ngspice characterization



#Cloning inverter from github repository

Git clone the files from GitHub to your local pc

```bash
 git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```



open another terminal and go to the location

```bash
/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic
```
copy the sky130A.tech file from this location to the git cloned location.


![Screenshot (180)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a0e190c2-60e7-42f2-8942-2cfa885747b8)


-Nmos

![Screenshot (181)](https://github.com/gsuni/VSD-Workshop/assets/99734954/151cfe81-92aa-4936-b90a-3aae7253a451)

-Pmos

![Screenshot (182)](https://github.com/gsuni/VSD-Workshop/assets/99734954/0a5b9f35-0796-4f68-9264-9beb281194ad)


![Screenshot (184)](https://github.com/gsuni/VSD-Workshop/assets/99734954/b1576251-0c68-470f-bcce-1b6cfed8e5f6)


#spice file

let's see what is inside the spice file by "vim sky130_inv.spice".

![Screenshot (186)](https://github.com/gsuni/VSD-Workshop/assets/99734954/c7ee2512-76bc-4396-81c5-da2691e5b6f4)


#Modifing the file

```bash

  option scale=0.01u 
include ./libs/pshort.lib
include ./libs/nshort.lib
//. subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort_model. 0w=37 l=23
+ ad=1443 pd=152 as=1517 ps=156
M1001 Y A VGND VGND nshort_model. 0 w=34 l=23
+ ad=1435 pd=152 as=1365
ps=148̦
VDD VPWR 0 3.3V
VSS VGND
0 OV
Va A VGND PULSE(OV 3.3V 0 0.1ns 0.1ns 2ns 4ns)
CO A Y 0.0754fF
C1 Y VPWR 0.117fF
C2 A VPWR 0.0774fF
C3 Y VGND 0.279fF
C4 A VGND 0.45fF
// C5 VPWR VGND 0.781f
//. ends 
.tran 1n 20n
.control run
endc
end
```

![Screenshot (187)](https://github.com/gsuni/VSD-Workshop/assets/99734954/17d59a33-a8e6-46e1-9e3d-2d49668d0ffc)

run the spice file in ngspice

```bash
 ngspice sky130_inv.spice
```

To plot the graph between Voltage and time type the command in ngspice

'plot y vs time a'

after running this file we get output of ngspice like this, 

![Screenshot (599)](https://github.com/gsuni/VSD-Workshop/assets/99734954/2636d88d-7310-459c-a252-73e571620f76)


![Screenshot (604)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a2ab1ac3-7ef9-4f8c-a474-f06aacac25b8)



-Increasing the value of C3 0.24ff to 2ff

![Screenshot (605)](https://github.com/gsuni/VSD-Workshop/assets/99734954/65603680-7f6f-4da5-b0fc-63d90855f48c)


![Screenshot (606)](https://github.com/gsuni/VSD-Workshop/assets/99734954/660ecfcc-80df-47c8-9368-acca644927a4)


-#20%

![Screenshot (607)](https://github.com/gsuni/VSD-Workshop/assets/99734954/dde73b0f-1948-4280-a0c9-91abfd425a45)

-#50%

![Screenshot (609)](https://github.com/gsuni/VSD-Workshop/assets/99734954/85271b77-16f5-4325-a56b-3694e9440933)


-#80%

![Screenshot (608)](https://github.com/gsuni/VSD-Workshop/assets/99734954/aad970df-6aef-4855-b97d-3191635f5844)

The value of parameters are:

- Rise time

rise time= (2.2489 - 2.181)e-09 = 66.92 psec

- Fall time

fall time= (4.0951 - 4.0526)e-09 = 42.51 psec

- Propagation delay


propogation delay =(2.2106 - 2.15012)e-09 = 60.48 psec

- Cell fall delay

cell fall delay =(4.07735 - 4.04988)e-09 = 27.47 psec

# DRC Corrections and rules

For doing DRC coorection we have to download lab files using

```bash
   wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```


To start magic tool,we can use command

```bash
  magic -d XR
```

And we will open 'met3.mag' file

![Screenshot (610)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d83f029a-908b-4314-abb3-d8adbbae68e2)

We can see contact cuts using:
```bash
cif see VIA2
```

![Screenshot (613)](https://github.com/gsuni/VSD-Workshop/assets/99734954/c3e16fd4-dde0-4cf7-a3c3-ec3fcd8df7f9)

![Screenshot (611)](https://github.com/gsuni/VSD-Workshop/assets/99734954/b0e2b398-bf4c-48f4-97fc-5ba02e6f01c5)

# Fixing ploy.9 error

![Screenshot (614)](https://github.com/gsuni/VSD-Workshop/assets/99734954/6743cc58-da78-4659-b049-ca82a815ca03)


![Screenshot (615)](https://github.com/gsuni/VSD-Workshop/assets/99734954/2f3b76bc-df0d-411f-a9e8-6ef81df4edb9)


![Screenshot (616)](https://github.com/gsuni/VSD-Workshop/assets/99734954/979cf75c-a5a2-4045-962f-b8eb30ebedac)

Now we run `tech loadsky130A.tech`  and do drc check using `check drc`


![Screenshot (618)](https://github.com/gsuni/VSD-Workshop/assets/99734954/5f0c984b-90f0-49df-89e5-59eb30b87e2a)


#  Lab challenge exercise to describe DRC error as geometrical construct



![Screenshot (619)](https://github.com/gsuni/VSD-Workshop/assets/99734954/28217fd9-df91-41bb-aaad-b2fe6526383a)


#Making some changes in sky130A.tech file


![Screenshot (620)](https://github.com/gsuni/VSD-Workshop/assets/99734954/6378a3c1-f455-43f8-a8e8-b04261de5849)



![Screenshot (621)](https://github.com/gsuni/VSD-Workshop/assets/99734954/04c1d150-a3a7-4b76-9d83-a542a56fc946)


Now executing the command `drc style drc(full)` and `drc check`


![Screenshot (622)](https://github.com/gsuni/VSD-Workshop/assets/99734954/4899d316-d86f-4540-83a5-05319da71dd7)



![Screenshot (623)](https://github.com/gsuni/VSD-Workshop/assets/99734954/f3cdf67b-4f38-491d-b2d2-254511fae825)

DAY 4: Pre-layout timing analysis and importance of good clock tree

#We are transforming grid data into lab data in this lab.

We can access the track file from the directory displayed in the picture to get additional details.


![Screenshot (625)](https://github.com/gsuni/VSD-Workshop/assets/99734954/202d0cc1-d237-43bd-b21a-d9c8279b33f3)


The track, which is essentially a trace of metal layers like metal 1, metal 2, etc., is utilized during the routing stage.

Since PNR is automated, we must designate the destinations for the trips. Tracks provide this specification. For layers li1, metal 1, and metal 2, each track is positioned at (0.23, 0.46)um in the horizontal direction and (0.17, 0.34)um in the vertical direction.

The ports are located on the li1 layer in the layout. We must translate the grid into the tracks in order to guarantee that the ports are at the intersection of the tracks.

We may accomplish this by opening the tracks file first, then the tkcon window and entering the help grid command.


![Screenshot (624)](https://github.com/gsuni/VSD-Workshop/assets/99734954/74322a53-a987-4f82-85ec-2d02dff12d00)



![Screenshot (626)](https://github.com/gsuni/VSD-Workshop/assets/99734954/90e6a5e3-4933-46d8-b88c-9561a957dbce)



As we can see, the ports have been positioned near the track intersection. However, three boxes are covered in between the lines. Therefore, this also fits our second requirement.

Lab steps to convert magic layout to std cell LEF

![Screenshot (627)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d1fb03b0-1e49-44ab-8292-6b0195b771ee)


![Screenshot (629)](https://github.com/gsuni/VSD-Workshop/assets/99734954/ca7fc2fa-1908-4576-8bf4-9398ef0ec985)


![Screenshot (630)](https://github.com/gsuni/VSD-Workshop/assets/99734954/61bff31b-785f-4e35-9eb6-a8145448159a)



now we will open this file in magic using
```bash
magic -T sky130A.tech sky130_vsdinv.mag &
```

now we have to run command lef write in takon window to extarct the lef file

![Screenshot (631)](https://github.com/gsuni/VSD-Workshop/assets/99734954/7ea30992-8a2c-4eea-86a3-b44247d02f4f)

we will need to move the files to the src folder where all design files are available.

To do this we can copy the file using 'cp' comman

![Screenshot (632)](https://github.com/gsuni/VSD-Workshop/assets/99734954/aa186324-86a4-4e8a-8c15-b42146bb5817)


OPENLANE :- Now we will go to the open lane directory and execute the docker command.

Will Execute the following commands in a line
```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

![Screenshot (633)](https://github.com/gsuni/VSD-Workshop/assets/99734954/24290733-3ba5-43d5-9faa-ee58183cfdba)


![Screenshot (634)](https://github.com/gsuni/VSD-Workshop/assets/99734954/89ed6def-0af1-45ec-a968-5f61a02c7dc5)




```bash
  run_synthesis
```

![Screenshot (635)](https://github.com/gsuni/VSD-Workshop/assets/99734954/21534ac3-d439-4052-9237-0c27e40b596a)


![Screenshot (636)](https://github.com/gsuni/VSD-Workshop/assets/99734954/65420fd2-5c03-4e23-ac31-44bda7c0fc73)

Commands to view and change parameters to improve timing and run synthesis

```tcl

prep -design picorv32a -tag 24-03_10-03 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

echo $::env(SYNTH_STRATEGY)

set ::env(SYNTH_STRATEGY) "DELAY 3"

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set ::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis
```
![Screenshot (8)](https://github.com/gsuni/VSD-Workshop/assets/99734954/b5bbd8a1-0032-47d4-a5c4-6f7c33d163f8)

![Screenshot (9)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a0e547c0-f0e8-4bca-a560-27b36bd740cc)

![Screenshot (10)](https://github.com/gsuni/VSD-Workshop/assets/99734954/0aa2852d-7b06-473e-bca6-498f65eb2c5f)

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command

```tcl
# Now we can run floorplan
run_floorplan
```
![Screenshot (11)](https://github.com/gsuni/VSD-Workshop/assets/99734954/52b1a7a5-1f33-4aef-a3fa-82d2eae414d8)

![Screenshot (12)](https://github.com/gsuni/VSD-Workshop/assets/99734954/25fc893c-b7a1-4e00-ac1c-0cdd92076a8f)

Since we are facing unexpected un-explainable error while using `run_floorplan` command, we can instead use the following set of commands

```tcl
init_floorplan
place_io
tap_decap_or
```
![Screenshot (13)](https://github.com/gsuni/VSD-Workshop/assets/99734954/81d32238-760a-489c-81c0-97b42f9f7e13)

```tcl
run_placement
```
![Screenshot (14)](https://github.com/gsuni/VSD-Workshop/assets/99734954/91152f6f-9e07-4f8f-aa23-1f36bac7e76c)

![Screenshot (15)](https://github.com/gsuni/VSD-Workshop/assets/99734954/9fe358c4-601a-4981-9065-1bcd83642278)

Abutment of power pins with other cell from library clearly visible

![Screenshot (16)](https://github.com/gsuni/VSD-Workshop/assets/99734954/128022f6-dd03-437e-9381-a6463a4486aa)

 Post-Synthesis timing analysis with OpenSTA tool.

 Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```
```tcl
./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_SIZING) 1

run_synthesis
```
![Screenshot (17)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d8a74e2b-e485-4c55-950c-2730ec9be756)

Newly created `pre_sta.conf` for STA analysis

![Screenshot (18)](https://github.com/gsuni/VSD-Workshop/assets/99734954/190c7652-497a-451b-99b4-d1117bd5c3e3)

Newly created `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src` directory

![Screenshot (19)](https://github.com/gsuni/VSD-Workshop/assets/99734954/24e34126-cbee-4738-9b37-e6494ded378d)


Commands to run STA in another terminal
```bash
cd Desktop/work/tools/openlane_working_dir/openlane

sta pre_sta.conf
```
![Screenshot (21)](https://github.com/gsuni/VSD-Workshop/assets/99734954/1f5f3575-3e32-40b3-b968-b77060e114e5)

![Screenshot (22)](https://github.com/gsuni/VSD-Workshop/assets/99734954/267c7bbf-96f9-4564-9aec-b51c87606cf7)

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis 

```tcl
prep -design picorv32a -tag 30-04_05-57 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_SIZING) 1

set ::env(SYNTH_MAX_FANOUT) 4

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis
```

![Screenshot (23)](https://github.com/gsuni/VSD-Workshop/assets/99734954/0ffedcbd-1ba8-49d9-9de9-c1885856b685)

![Screenshot (24)](https://github.com/gsuni/VSD-Workshop/assets/99734954/c3073bbe-a78d-4bf6-b4a5-b97e9276e443)

```bash
cd Desktop/work/tools/openlane_working_dir/openlane

sta pre_sta.conf
```
![Screenshot (25)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a5137935-52ab-4c4c-abcd-009369b932fe)


Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
report_net -connections _11672_

help replace_cell

replace_cell _14510_ sky130_fd_sc_hd__or3_4

report_checks -fields {net cap slew input_pins} -digits 4
```
![Screenshot (26)](https://github.com/gsuni/VSD-Workshop/assets/99734954/01353eca-1298-434c-abf0-48d47849d594)


![Screenshot (27)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a236e115-0f4a-4b37-a6f6-e670ee69e5dd)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
report_net -connections _11675_

replace_cell _14514_ sky130_fd_sc_hd__or3_4

report_checks -fields {net cap slew input_pins} -digits 4
```

![Screenshot (28)](https://github.com/gsuni/VSD-Workshop/assets/99734954/0af8c054-cb7f-44ad-98c1-85bf4673ee62)


![Screenshot (29)](https://github.com/gsuni/VSD-Workshop/assets/99734954/48ade5a0-3040-4d68-a68f-0c9d6e9293d9)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
report_net -connections _11643_

replace_cell _14481_ sky130_fd_sc_hd__or4_4

report_checks -fields {net cap slew input_pins} -digits 4
```

![Screenshot (30)](https://github.com/gsuni/VSD-Workshop/assets/99734954/1eea471d-d726-4011-8cb1-6c9134c17b05)


![Screenshot (31)](https://github.com/gsuni/VSD-Workshop/assets/99734954/bb5e407d-a035-4b24-8b05-65ad51fe6276)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
report_net -connections _11668_

replace_cell _14506_ sky130_fd_sc_hd__or4_4

report_checks -fields {net cap slew input_pins} -digits 4
```

![Screenshot (32)](https://github.com/gsuni/VSD-Workshop/assets/99734954/213718d8-4d6c-4b65-b924-3758d00908d9)

![Screenshot (33)](https://github.com/gsuni/VSD-Workshop/assets/99734954/1bb69221-9207-4c26-9500-03255e0320db)

```tcl
report_checks -from _29043_ -to _30440_ -through _14506_
```

![Screenshot (34)](https://github.com/gsuni/VSD-Workshop/assets/99734954/cbe68ba7-be97-4578-9367-08c1582ac9b7)

![Screenshot (35)](https://github.com/gsuni/VSD-Workshop/assets/99734954/c773df43-2425-4be0-a5a0-0b8fd8bca221)

Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Commands to make copy of netlist

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-04_05-57/results/synthesis/

cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```

![Screenshot (36)](https://github.com/gsuni/VSD-Workshop/assets/99734954/ae662513-0eb1-4942-8b31-4d6aba0fcff3)

Commands to write verilog

```tcl
help write_verilog

write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v

exit
```

![Screenshot (37)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d61605d8-7f11-4316-a28b-8ed66e9c2c5e)

Commands load the design and run necessary stages

```tcl
prep -design picorv32a -tag 30-04_05-57 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_STRATEGY) "DELAY 3"

set ::env(SYNTH_SIZING) 1

run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement

unset ::env(LIB_CTS)

run_cts
```
![Screenshot (38)](https://github.com/gsuni/VSD-Workshop/assets/99734954/733440ed-369e-4a53-9f80-471c83b54610)


![Screenshot (39)](https://github.com/gsuni/VSD-Workshop/assets/99734954/789b99a9-21f1-4ba6-9308-9e089b4afb45)


![Screenshot (40)](https://github.com/gsuni/VSD-Workshop/assets/99734954/928f7552-b166-45f4-badd-f600b5cbadb9)


![Screenshot (41)](https://github.com/gsuni/VSD-Workshop/assets/99734954/e5815adc-c337-4057-bfcd-f2cb0d2b68b8)


![Screenshot (42)](https://github.com/gsuni/VSD-Workshop/assets/99734954/ee50358e-f594-407a-8649-2d2ef9b0ed2b)

Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
openroad

read_lef /openLANE_flow/designs/picorv32a/runs/30-04_05-57/tmp/merged.lef

read_def /openLANE_flow/designs/picorv32a/runs/30-04_05-57/results/cts/picorv32a.cts.def

write_db pico_cts.db

read_db pico_cts.db

read_verilog /openLANE_flow/designs/picorv32a/runs/30-04_05-57/results/synthesis/picorv32a.synthesis_cts.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

set_propagated_clock [all_clocks]

help report_checks

report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

exit
```

![Screenshot (43)](https://github.com/gsuni/VSD-Workshop/assets/99734954/cc690545-64e3-4161-a219-abb53b2c492e)

![Screenshot (44)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d2014418-23fc-4e53-a3d0-4960944d101e)



Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing `CTS_CLK_BUFFER_LIST`

```tcl
echo $::env(CTS_CLK_BUFFER_LIST)

set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

echo $::env(CTS_CLK_BUFFER_LIST)

echo $::env(CURRENT_DEF)

set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/30-04_05-57/results/placement/picorv32a.placement.def

run_cts

echo $::env(CTS_CLK_BUFFER_LIST)

openroad

read_lef /openLANE_flow/designs/picorv32a/runs/30-04_05-57/tmp/merged.lef

read_def /openLANE_flow/designs/picorv32a/runs/30-04_05-57/results/cts/picorv32a.cts.def

write_db pico_cts1.db

read_db pico_cts.db

read_verilog /openLANE_flow/designs/picorv32a/runs/30-04_05-57/results/synthesis/picorv32a.synthesis_cts.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

set_propagated_clock [all_clocks]

report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

report_clock_skew -hold

report_clock_skew -setup

exit

echo $::env(CTS_CLK_BUFFER_LIST)

set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

echo $::env(CTS_CLK_BUFFER_LIST)
```

![Screenshot (45)](https://github.com/gsuni/VSD-Workshop/assets/99734954/4308de18-52d0-419f-86d3-55617fcc32ce)


![Screenshot (46)](https://github.com/gsuni/VSD-Workshop/assets/99734954/4818470b-a488-4963-b3f2-6f50ff888182)


![Screenshot (47)](https://github.com/gsuni/VSD-Workshop/assets/99734954/d7fc5157-df30-45ef-a962-94aacd0817af)


![Screenshot (48)](https://github.com/gsuni/VSD-Workshop/assets/99734954/94b30be7-55cc-4cd7-aff7-37300bcb30dc)


Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now

```bash
cd Desktop/work/tools/openlane_working_dir/openlane

docker
```

```tcl
./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_STRATEGY) "DELAY 3"

set ::env(SYNTH_SIZING) 1

run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement

run_cts

gen_pdn 
```

![Screenshot (49)](https://github.com/gsuni/VSD-Workshop/assets/99734954/88bfea04-7871-43a8-8497-c47bfe2ec6b9)


![Screenshot (50)](https://github.com/gsuni/VSD-Workshop/assets/99734954/71896aa4-506f-435b-a6fd-7f323f5ca505)


![Screenshot (51)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a06ef2b4-25dd-4e54-b155-84f904a66dec)

Commands to load PDN def in magic in another terminal

```bash

cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/02-05_04-43/tmp/floorplan/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

![Screenshot (52)](https://github.com/gsuni/VSD-Workshop/assets/99734954/ae69c47e-b40e-40db-a986-c7929ae6e750)


![Screenshot (53)](https://github.com/gsuni/VSD-Workshop/assets/99734954/5a7b4043-9597-47d6-9c3c-baa104b1d83f)

Command to perform routing

```bash
echo $::env(CURRENT_DEF)

echo $::env(ROUTING_STRATEGY)

run_routing
```

![Screenshot (54)](https://github.com/gsuni/VSD-Workshop/assets/99734954/a77eaa37-e345-4866-9f3b-4f14946ed390)

Commands to load routed def in magic in another terminal

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/02-05_04-43/results/routing/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

![Screenshot (55)](https://github.com/gsuni/VSD-Workshop/assets/99734954/aeb1a92c-1936-4199-9536-3556b5bea682)


![Screenshot (56)](https://github.com/gsuni/VSD-Workshop/assets/99734954/9089dc0e-c99c-4541-a5a5-1f8be808589e)


![Screenshot (57)](https://github.com/gsuni/VSD-Workshop/assets/99734954/efffcc03-aa69-437d-be60-8f3669280543)


Screenshot of fast route guide


![Screenshot (58)](https://github.com/gsuni/VSD-Workshop/assets/99734954/7e745912-b94c-4ecd-ac6d-1b6cf69bc61c)


This is the final generated layout


![Screenshot (59)](https://github.com/gsuni/VSD-Workshop/assets/99734954/87843918-793c-4cb1-a799-5caaf46a9ea7)

