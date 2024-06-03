# VSD_SoC_Design-Planning
## Setup
brew || UTM || Loading etc.
## Day1
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
### Flop Ratios
