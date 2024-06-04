# VSD_SoC_Design-Planning

## RTL2DGSII
The RTL2GDSII flow, also known as the digital or ASIC flow, is the journey of transforming an idea into a physical microchip. It's a multi-stage process that translates a designer's concept, written in a hardware description language (HDL) like Verilog, into a GDSII file. This file acts as the blueprint for the chip's fabrication in a foundry.
This flow encompasses various stages, each crucial for ensuring the chip functions correctly and can be manufactured efficiently. We'll delve into these stages, from design verification and logic translation to physical layout and final GDSII generation. Buckle up, as we explore the exciting world of turning ideas into silicon!

## OpenLane
OpenLane emerges as an innovative open-source platform geared towards simplifying and automating the RTL (Register-Transfer Level) to GDSII (Physical Design Interchange Format) flow. This flow sits at the heart of integrated circuit (IC) design, translating a designer's concept into the blueprint used to manufacture the chip.
Traditionally, this multi-step process can be complex and involve various tools. OpenLane stands out by offering an open-source infrastructure that seamlessly integrates established tools like Yosys, OpenROAD, and Magic. This not only empowers designers with greater control and flexibility but fosters collaboration within the chip design community.

<img width="856" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/ada46e59-25c6-49a4-b2ca-62b9ed74cda5">

## Tools

* Yosys - Synthesis of RTL Design
* ABC - Mapping of Netlist
* OpenSTA - Static Timing Analysis
* OpenROAD - Floorplanning, Placement, CTS, Optimization, Routing
* TritonRoute - Detailed Routing
* Magic VLSI - Layout Tool
* NGSPICE SPICE - Extraction and Spice Simulation
* SPEF_EXTRACTOR - Generation of SPEF file from DEF file

## Setup
If you are Using ARM based MAC: Virtualbox is not supported to open .vdi files.
You can use UTM and follow below to videos to emulate qcow2 files.

* https://sysadmin102.com/2024/01/utm-converting-vdivirtualbox-raw-vmdkvmware-image-to-qemu-image-qcow2/
* https://www.youtube.com/watch?v=g60Xr9dL3pc
  
## Day 1
### 1. Opening OpenLane and Running Synthesis

* Start OpenLane Flow
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

* Prepare the design for run and run Synthesis.

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
### 2. Calculate Flop Ratios

* Open Synthesis Report
  
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
* Locate Floorplan.def

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

#### A. Equidistant Placement of Ports & Diagonally Equidistant Tap Cells

<img width="1230" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/48e66f2c-1084-410c-849b-b2579a67a584">

#### B. Unplaced Standard Cell at Origin
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

* Legally placed standard cells
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


### 2. Spice Extractation of Inverter into Magic
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
* Observing Units
<img width="658" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/56ce92fe-463a-407a-b472-fdbfffc20ebd">

* Edited Spice model file
<img width="907" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/7219f434-700f-43e8-8d1e-249bc722dbb5">

### 4. Post layout Ngspice Simulations

```
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```
<img width="1209" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/80c45ab6-0672-4974-b525-27f247c1fa1d">

* Ngspice Waveform
<img width="1065" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/ba16caef-aaa3-4dda-bd5a-d12a3e9ec402">

#### Calculations
- Rise time :
It is time taken to the output waveform to 20% value to 80% value.

  so, rise time = (2.2476 - 2.1821)e-09 = 65.52 psec.

- Fall time :
It is the time take by output for transition from 80% to 20%.

  so, fall time = (4.09531 - 4.05312)e-09 = 42.19 psec.
  
- Rise cell delay calculation:
  It is time for output is rising to 50% and input is falling to 50%

  so, rise cell delay = (2.21102 - 2.1501)e-09 = 60.92 psec.
    
- Fall cell delay calculation :
It is time for output falling to 50% and input is rising to 50%.

  so, fall cell delay = (4.073675 - 4.04994)e-09 = 23.74 psec.




### 5. Fixing DRC in old magic tech file
```
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# Command to open magic tool in better graphics
magic -d XR &
```

* Link to Sky130 Periphery rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

#### A. Open poly.mag

<img width="1229" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/075c2a46-be1b-4842-9821-9394968333fa">

#### B. Incorrectly implemented poly
<img width="696" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/49719379-4cdf-4ba1-9860-8f4b60f5d90b">
<img width="773" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/a56297e0-1df9-43ee-a598-ff70522f7306">

```
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

<img width="955" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/25b5f6e8-affa-4911-aefb-294b468b6b8c">

#### C. nwell: DRC error as geometrical construct

```
cif ostyle drc
cif see dnwell_shrink
feed clear
cif see nwell_missing
feed clear
```

<img width="589" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/1465307d-67fe-4f3a-a66d-83d0a1c755bb">
<img width="508" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/54988c6a-03be-4a33-b0d8-d4a2707e00cb">

```
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```
<img width="793" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/12de0eb7-dd13-49ab-9f62-43610f8c33cd">

## Day 4

### 1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

```
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &

# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um

```
#### A. The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks. (Verified)

<img width="697" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/5f3aaaec-c900-4d51-b6cd-917ed6ae6da8">

#### B. Width of the standard cell should be odd multiples of the horizontal track pitch. & Height of the standard cell should be even multiples of the vertical track pitch. (verified)

Width = 3 * 0.46
Height = 8 * 0.34

<img width="686" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/324e85d7-f725-4f78-84a0-d4843131ca28">

```
# Command to save as custom name
save sky130_shaninv.mag

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_shaninv.mag &

# Write lef
lef write
```
<img width="962" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/1f5fc7a1-e257-46c0-b973-74c0988a617e">

### 2. Copy Custom Inverter and Update config
#### A. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

```
# Copy lef file
cp sky130_shaninv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

#### B. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow
<img width="1091" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/f2d0fad9-1a79-44c5-a78e-76b73a5e1363">

### 3. Run openlane flow with newly inserted customer inverter cell
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

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
<img width="1223" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/56291d05-1754-4e16-afa4-e4e81ba1a81c">
<img width="1134" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/60d55785-1421-4b2d-97dc-84f04eb1609f">

### 4. Fixing the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 04-06_07-47 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
<img width="1089" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/e0b6d1a9-6686-4fcd-837d-dc931339d0d6">

* Fixed Violation
<img width="1212" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/16bb64ad-d8f9-4a6c-b499-05388b5987dd">

#### A. Run Floorplan and Placement
```
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

```
<img width="1221" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/d121a2c0-cf53-4c79-bb09-2a1149ce6dd7">

```
# Now we are ready to run placement
run_placement

```
<img width="1226" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/9cf223b7-bf15-46f3-afac-f5f11ac41c31">

#### B. Check the Placement in Magic and also Custom inverter

```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/04-06_07-47/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

<img width="1006" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/c9eb6cf2-cc20-47e1-84dc-8af87b90f60b">

### INV and abutted cell

### 5. Post-synthesis timing analysis: OpenSTA
#### A. Create pre_sta.conf
<img width="1015" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/7b92e708-07df-4a57-94eb-c47ac5828a4d">

#### B. Create my_base.sdc
<img width="949" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/af711343-d880-472e-8321-e3090c3c72bb">

#### C. Run STA

```
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
<img width="1231" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/67615bc3-831c-424f-8e92-96f51cbb0e2a">

#### * Since here slack is already met. No need to do ECO fixes.

### 6. Run CTS

```
# With placement done we are now ready to run CTS
run_cts
```
<img width="1232" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/37504b3c-6fe2-4914-bb68-78108e469c1e">

### 7. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/04-06_07-47/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/04-06_07-47/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/04-06_07-47/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

<img width="846" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/e167817a-d358-4206-b237-d9b8f3ca119f">

#### A. Execute OpenSTA with right timing libraries and CTS by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

```
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/04-06_07-47/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```
<img width="1227" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/3e34e8a8-d912-4bcc-be58-f5705bfd6859">

#### B. Observe impact of Bigger CTS Buffer

```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/04-06_07-47/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/04-06_07-47/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/04-06_07-47/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit
```
<img width="1226" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/db3ce286-3426-479a-bb71-6e76ac2bbb95">

```
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```
<img width="1220" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/bb649673-7c13-4d27-92d2-ffd26ca78a57">

## Day 5

### 1. Generate Power Distribution Network (PDN) and explore the PDN layout.

```
# Now that CTS is done we can do power distribution network
gen_pdn

# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/04-06_07-47/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 23-pdn.def &
```
<img width="1224" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/6e3da5e7-9912-459d-9558-77ac7fc7e66e">
<img width="1226" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/a3ded740-0744-4055-a808-6ff5126706f8">
<img width="1224" alt="image" src="https://github.com/2107shantanu/VSD_SoC_Design-Planning/assets/54627896/2a48c107-33c6-49ac-a98c-bf4183275e90">

### 2. Perfrom detailed routing using TritonRoute and explore the routed layout.

```
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```


## Refrences
1. Magic DRC: http://opencircuitdesign.com/magic/Technologyfiles/TheMagicTechnologyFileManual/DrcSection
2. Google Skywater Design Rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html
3. Google Skywater PDK: https://github.com/google/skywater-pdk
4. Ebaless - OpenLane: https://efabless.com/openlane
5. Magic: http://www.opencircuitdesign.com/magic/index.html
6. Inverter design: https://github.com/nickson-jose/vsdstdcelldesign/tree/master





