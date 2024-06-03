# VSD_SoC_Design-Planning
## Setup
brew || UTM || Loading etc.
## Day 1
### 1. Opening OpenLane and Running Synthesis

```
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9
```

<img width="1212" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/2fb7a8d6-173d-464b-b477-5fe40e1ef0fe">

```
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
<img width="1229" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/b73c09de-a129-490e-874c-550da44d8497">


```
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
### 2. Flop Ratios

```
# Open new terminal tab
# Change to latest synthesis report directory and view synthesis statistic report 

cd /designs/picorv32a/runs/03-06_12-06/report/synthesis
gvim 1-yosys_4.stat.rpt 
```

<img width="996" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/5ce511c4-6c43-4e98-afe8-af7b8e794d4e">

<img width="845" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/349ca380-f8d4-4ce2-9bf0-5217862bab44">

## Day 2

### 1. Run Floorplan
```
# Run floorplan command
run_floorplan

```
<img width="1219" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/5237fb78-e353-4a2e-ae42-2fb9531f33a5">

### 2. Calculate Die Area
```
#Locate Design Floorplan.def
gvim Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-06_12-06/results/floorplan

```
<img width="989" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/61e41057-9cd3-4959-ac85-d7d2c85ed825">
<img width="791" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/24bcdeb6-8a72-4d91-b7ce-1d2df766206f">

### 3. Load Magic Tool & Explore Floorplan
```
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-06_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

```
<img width="1228" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/12408a42-8079-4ce4-8c19-95ee874b8a34">

#### A. Equidistant Placement of Ports
#### B. Diagonally Equidistant Tap Cells

<img width="1230" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/48e66f2c-1084-410c-849b-b2579a67a584">

#### C. Unplaced Standard Cell at Origin
<img width="1222" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/ab5341c1-a221-4e77-ac1a-256ac571abf2">

### 4. Run Placement
```
run_placement
```
<img width="1225" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/80e5cbfb-abbb-46d7-ab4a-c0d0f02b2e33">

### 5. Load Magic Tool & Explore Placement

```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-06_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
<img width="967" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/b3b00e16-e917-47b7-8eff-b8a5b05bbc32">

#### Legally placed standard cells
<img width="971" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/43c7c15e-215f-48db-aa6c-2d65741f49ac">

```
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

## Day 3

