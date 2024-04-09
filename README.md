# VSD-Workshop


Day-1 Inception of open source,EDA Openlane and Sky130 PDK
##Using following command to invoke openlane:<br>
```bash
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
```
![1](https://github.com/gsuni/VSD-Workshop/assets/99734954/f944fc9f-4ade-4ba7-806b-486422802fee)
### Synthesis
   
 To run the Synthesis
   
    ```bash
       run_synthesis
    ```
![2](https://github.com/gsuni/VSD-Workshop/assets/99734954/4f9bdf6b-8829-461a-8e80-896f4a387fd9)

#To calculate the  flip-flop ratio

![3](https://github.com/gsuni/VSD-Workshop/assets/99734954/b6a9d016-a720-4d1b-9e0f-7d7f0957fa1c)

![4](https://github.com/gsuni/VSD-Workshop/assets/99734954/0356851d-795d-4464-8541-aa732038f6ca)

we are interested in finding the Flop ratio. This can be calculated by using the formula:
   
       Flop Ratio = No. of D FlipfLops/Total No. of cells
      
                   =1613/14876
      
                   =0.10842
      
        Percentage of D FF's = 10.842%

 ## Day2 Good floorplan vs bad floorplan and introduction to library cells
   
 ## Floorplanning and Intro to Library Cells
 
```bash
  run_floorplan
```

![5](https://github.com/gsuni/VSD-Workshop/assets/99734954/a3af31bb-996d-4af2-a1f9-b8e4d2f79592)

![6](https://github.com/gsuni/VSD-Workshop/assets/99734954/cd46191b-a955-4fbf-92fd-471ed614aacf)


To see the actual layout after the flow, we have to open the magic file by adding the command

```bash
magic -T /home/vsdsquadron/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef 
  read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![7](https://github.com/gsuni/VSD-Workshop/assets/99734954/36f44956-6797-4eef-bd8d-43a2eff44cb2)

![8](https://github.com/gsuni/VSD-Workshop/assets/99734954/e79bcaec-38c1-43c8-9c7b-92ce02d9b517)

![9](https://github.com/gsuni/VSD-Workshop/assets/99734954/1ce70a1a-e075-4ec7-8fbc-772dfc9db852)

Horizontal metal layer

![10](https://github.com/gsuni/VSD-Workshop/assets/99734954/228ad26c-f036-487b-a8e4-020922edf89e)

Vertical metal layer

![11](https://github.com/gsuni/VSD-Workshop/assets/99734954/00a4f160-8a55-402d-b1f9-642cc0fd3f3a)

To run the Placement, the command is
```bash
run_placement
```
![12](https://github.com/gsuni/VSD-Workshop/assets/99734954/6be348b4-3c9a-4a90-aec4-c641feb66881)


The Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below.  
  Commands to load placement def in magic
  
  ```bash
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![13](https://github.com/gsuni/VSD-Workshop/assets/99734954/91a6f682-352c-4e92-b096-3d1d32cff7cc)

Screenshot of custom inverter inserted in placement def with proper abutment

![14](https://github.com/gsuni/VSD-Workshop/assets/99734954/a991bf9f-d6bf-4145-8213-565d787e4252)

- Vertical metal layer
  
![15](https://github.com/gsuni/VSD-Workshop/assets/99734954/b5fefa30-afe6-49be-921c-05268309dff3)

- Horizontal metal layer
  
![16](https://github.com/gsuni/VSD-Workshop/assets/99734954/4a8a7b2c-4955-44df-8cd4-bc8eb80d83c2)

DAY 3: Design library cell using Magic Layout and ngspice characterization



























    







