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

### 1. Clone, Load the custom inverter layout in magic and explore.
```
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
# custom inverter standard cell design from github repository: Standard cell design and characterization using OpenLANE flow.
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```
<img width="967" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/0879d883-ace1-47d8-8db5-81b64fb29e9d">

#### A. NMOS (Verified)

<img width="834" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/dc425e8f-3ee3-4771-ade3-522573521e00">

#### B. PMOS (Verified)
<img width="767" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/ef344207-9c70-4897-96a6-1dfb6eb827b5">

#### C. Output Y connection to PMOS & NMOS drain (Verified)
<img width="383" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/1c822ca3-4a8d-4b15-8f59-25663cc05420">

#### D. Source of PMOS connected to VDD (Verified)
<img width="296" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/bd7995c3-0746-4314-bac6-d0a5bb023ca2">

#### E. Source of NMOS connected to GND (Verified)
<img width="307" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/5c158100-4d73-4334-8219-5f1c91173a76">

#### F. Creating DRC Error
<img width="838" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/d1832594-751a-4d9b-b667-c0dcad0ae867">


### 2. Spice Extractor of Inverter into Magic
```
#Enter these commands in tkcon window of magic
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
<img width="910" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/98ae65ab-0938-4f1f-86da-68b732d433f9">

### 3. Editing Spice model file for analysis through simulation.
#### Units
<img width="658" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/56ce92fe-463a-407a-b472-fdbfffc20ebd">






