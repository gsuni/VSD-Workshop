
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

![28](https://github.com/gsuni/VSD-Workshop/assets/99734954/e61b3877-2583-40a8-bcd5-055d19e7e8fe)


![29](https://github.com/gsuni/VSD-Workshop/assets/99734954/4fdc0e53-9f97-4360-a9dc-1f57637cc238)

-Increasing the value of C3 0.24ff to 2ff

![30](https://github.com/gsuni/VSD-Workshop/assets/99734954/c301cd25-7812-4580-af9c-aab39b1a6c74)

-#20%

![31](https://github.com/gsuni/VSD-Workshop/assets/99734954/b27e5921-2924-44c1-99ec-d035fe259d64)

-#50%

![33](https://github.com/gsuni/VSD-Workshop/assets/99734954/a95f64ea-a799-4d14-bb80-9d0edb6d9b6c)


-#80%

![32](https://github.com/gsuni/VSD-Workshop/assets/99734954/18bf4151-a39b-40cb-bb4c-757a2290693b)

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

![33](https://github.com/gsuni/VSD-Workshop/assets/99734954/495f11bb-7199-4ac2-bbfc-cf195b67bf94)

To start magic tool,we can use command

```bash
  magic -d XR
```

And we will open 'met3.mag' file

![34](https://github.com/gsuni/VSD-Workshop/assets/99734954/7a1d604f-f9eb-46e1-a774-35334c353334)

We can see contact cuts using:
```bash
cif see VIA2
```
![35](https://github.com/gsuni/VSD-Workshop/assets/99734954/172d5019-7ed2-4135-8ac4-c08e112d6602)

# Fixing ploy.9 error

![36](https://github.com/gsuni/VSD-Workshop/assets/99734954/eb327277-9a35-4416-8463-9e559402319f)


![37](https://github.com/gsuni/VSD-Workshop/assets/99734954/d2309835-f2e2-4975-bdcb-8c2e0dad564e)

Now we run `tech loadsky130A.tech`  and do drc check using `check drc`


![38](https://github.com/gsuni/VSD-Workshop/assets/99734954/e2208a81-d333-4eec-bd15-20ff85cca63f)


#  Lab challenge exercise to describe DRC error as geometrical construct



![39](https://github.com/gsuni/VSD-Workshop/assets/99734954/5c3b2e9c-ad36-4462-aed4-b216da7ea58f)


#Making some changes in sky130A.tech file


![40](https://github.com/gsuni/VSD-Workshop/assets/99734954/dc5db316-348e-441f-8ad4-17778372b4fb)



![41](https://github.com/gsuni/VSD-Workshop/assets/99734954/17934f25-8a09-4628-8602-de3e7855de39)


Now executing the command `drc style drc(full)` and `drc check`


![42](https://github.com/gsuni/VSD-Workshop/assets/99734954/3eaba139-ea50-4a94-823d-70fb046729e9)



![43](https://github.com/gsuni/VSD-Workshop/assets/99734954/2dfd6cc3-7f17-4f19-911c-29d49ec8971d)



![44](https://github.com/gsuni/VSD-Workshop/assets/99734954/71089d78-8c84-4514-8d5f-788408424c84)

DAY 4: Pre-layout timing analysis and importance of good clock tree

#We are transforming grid data into lab data in this lab.

We can access the track file from the directory displayed in the picture to get additional details.

![45](https://github.com/gsuni/VSD-Workshop/assets/99734954/29c218c2-cb08-43b0-931b-5580f4c69965)

The track, which is essentially a trace of metal layers like metal 1, metal 2, etc., is utilized during the routing stage.

Since PNR is automated, we must designate the destinations for the trips. Tracks provide this specification. For layers li1, metal 1, and metal 2, each track is positioned at (0.23, 0.46)um in the horizontal direction and (0.17, 0.34)um in the vertical direction.

The ports are located on the li1 layer in the layout. We must translate the grid into the tracks in order to guarantee that the ports are at the intersection of the tracks.

We may accomplish this by opening the tracks file first, then the tkcon window and entering the help grid command.


![46](https://github.com/gsuni/VSD-Workshop/assets/99734954/c5044940-e6b0-4c53-a8fa-64a75046a342)


![47](https://github.com/gsuni/VSD-Workshop/assets/99734954/b761e50a-8868-48eb-acb1-3c663de941f1)


![48](https://github.com/gsuni/VSD-Workshop/assets/99734954/a07beec5-ad25-4eb3-95bb-1077b049bf28)


As we can see, the ports have been positioned near the track intersection. However, three boxes are covered in between the lines. Therefore, this also fits our second requirement.

Lab steps to convert magic layout to std cell LEF

![49](https://github.com/gsuni/VSD-Workshop/assets/99734954/da5659c0-108d-47be-9736-9e7b6740d227)


![500](https://github.com/gsuni/VSD-Workshop/assets/99734954/4cbd330f-3b32-4b2f-acb3-9174e0e086d3)


now we will open this file in magic using
```bash
magic -T sky130A.tech sky130_vsdinv.mag &
```

now we have to run command lef write in takon window to extarct the lef file

![51](https://github.com/gsuni/VSD-Workshop/assets/99734954/ae281bbf-1aee-444a-935a-fd75a395a017)

we will need to move the files to the src folder where all design files are available.

To do this we can copy the file using 'cp' comman

![52](https://github.com/gsuni/VSD-Workshop/assets/99734954/c402c501-2783-4e7f-bf62-bad68b847130)


![53](https://github.com/gsuni/VSD-Workshop/assets/99734954/f0acf5fa-f6b3-4456-adb5-998af7c34879)

OPENLANE :- Now we will go to the open lane directory and execute the docker command.

Will Execute the following commands in a line

#OPENLANE

Now we will open openlane directry and run 'docker' command

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

run_synthesis

prep -design picorv32a -tag 08-04_18-39 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs


![54](https://github.com/gsuni/VSD-Workshop/assets/99734954/5e2b59c5-6d17-4a94-9200-71405fa6358c)


![55](https://github.com/gsuni/VSD-Workshop/assets/99734954/9886d966-f429-4fa1-93e6-f90291defa73)


```bash
  run_synthesis
```

![56](https://github.com/gsuni/VSD-Workshop/assets/99734954/de61c672-dd8d-4695-9788-5994d0684823)


#adjusting synthesis parameters to incorporate vsdinv and fix slack

The README.MD file in the configuration directory will be changed.


![57](https://github.com/gsuni/VSD-Workshop/assets/99734954/361977c0-b66b-46fe-abce-54937df15fb3)


Now we will run the following commands in openlane:


`echo $::env(SYNTH_STRATEGY)`

`set ::env(SYNTH_STRATEGY) "DELAY 3"`

`echo $::env(SYNTH_BUFFERING)`

`echo $::env(SYNTH_SIZING)`

`set ::env(SYNTH_SIZING) 1`

`echo $::env(SYNTH_DRIVING_CELL)`

`run_synthesis`


![58](https://github.com/gsuni/VSD-Workshop/assets/99734954/490fd4b4-4a55-49f4-8b08-1b049d1ec94c)


```bash
 run_synthesis
```

![59](https://github.com/gsuni/VSD-Workshop/assets/99734954/0fcf07c7-c998-4e21-a93c-80186e45b1fd)


![60](https://github.com/gsuni/VSD-Workshop/assets/99734954/02b760e2-b44b-4adf-907a-b762bfdf68a9)

#To resolve the issue

'init_floorplan'

'place_io'

'tap_decap_or'

![61](https://github.com/gsuni/VSD-Workshop/assets/99734954/e04ec5b9-80a4-4480-9a0c-f18e5275d41c)


![62](https://github.com/gsuni/VSD-Workshop/assets/99734954/6728d03c-4643-4f6e-8615-eeffe1a42f97)


![63](https://github.com/gsuni/VSD-Workshop/assets/99734954/383d8bb4-3f3e-409d-8473-cb06d49b62b2)


Now that errors have been successfully fixed, the 'run_placement' command is being used.


![64](https://github.com/gsuni/VSD-Workshop/assets/99734954/dffeee1e-d709-4a39-b2db-e7bdae083777)

```bash
  
   magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged_unpadded.lef def read picorv32a.floorplan.def &
```

![65](https://github.com/gsuni/VSD-Workshop/assets/99734954/aaf63fc9-0731-4044-a23f-23533b897918)


#placement output in magic

![66](https://github.com/gsuni/VSD-Workshop/assets/99734954/31c5ce3a-6a4b-41a8-a7f1-81f1b767f5d7)


To view internal layers we need to run command 'expand' in tckon tab

![67](https://github.com/gsuni/VSD-Workshop/assets/99734954/3981813b-76bf-46a3-8d86-10a8bcc68c59)


# DAY 5:  Final steps for RTL2GDS using tritonRoute and openST

#performng PDN generation and layout

 Building power distribution network

` docker`

`./flow.tcl -interactive`

`package require openlane 0.9`

`prep -design picorv32a -tag 08-04_18-39`

`echo $::env(CURRENT_DEF)`


![68](https://github.com/gsuni/VSD-Workshop/assets/99734954/f6775d09-c63f-4784-ba9f-bcab011a551b)


![69](https://github.com/gsuni/VSD-Workshop/assets/99734954/6007df83-41f2-4177-a373-715060196981)


#Generating pdn(power distribution network) using `gen_pdn`


![70](https://github.com/gsuni/VSD-Workshop/assets/99734954/ad2d6966-9b24-4683-ae67-d89eb93afed5)


#Routing

 Running `echo $::env(CURRENT_DEF)` and ` echo $::env(ROUTING_STRATEGY)`
 
 
![71](https://github.com/gsuni/VSD-Workshop/assets/99734954/4738d6dc-9b85-472a-bb61-25a4842a7082)


 ```bash
   run_routing
```

![72](https://github.com/gsuni/VSD-Workshop/assets/99734954/86e194b5-57f3-452f-bdfd-1a3215ff409b)


OpenLANE lacks a SPEF extraction tool, hence this procedure must be completed externally to OpenLANE.

You may find the generated.spef file in the routing folder, which is situated beneath the results folder.


![73](https://github.com/gsuni/VSD-Workshop/assets/99734954/423e281a-cd3e-4c71-855a-0c6485d5f396)


![74](https://github.com/gsuni/VSD-Workshop/assets/99734954/2a5455b3-341d-48ad-952a-f046af231835)


#Final layout
    

![75](https://github.com/gsuni/VSD-Workshop/assets/99734954/8dd2a87d-aea5-42f0-83b4-3138ce2349c8)






