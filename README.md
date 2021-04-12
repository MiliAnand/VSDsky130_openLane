# OpenLANE
OpenLANE is an automated RTL2GDSII flow which comprises many other EDA tools out there, for eg. Yosys, OpenSTA, Netgen, Magic etc. The main idea of OpenLANE is to have a complete and clean RTL2GDSII flow without any human intervention. OpenLANE is tuned for Skywatrer 130nm opensource PDK and can be used to produce hard macros and chips.

## Introduction
OpenLANE ASIC flow has several steps. The flow starts with design RTL and ends with final layout in GSII format. Foe functioning it needs PDKs. OpenLANE is based on several opensource projects such as Magic, Yosys, qflow, Fault, openroad, abc and Klayout. Here is the ASIC flow.

![](DAY1/openlane.flow.1.png)

* The flow starts with RTL sysnthesis. RTL is fed to *yosys* with some design constraints. *yosys* basically translates the RTL into a logic circuit using engineering components. This circuit is optimised and then mapped into cells using *abc*. *abc* has to be guided during the optimisation and that is done by abc script. OpenLANE comes with various abc scripts, we refer to them as synthesis strategies. We have strategies that target least area and we have strategies that targets best timings. Different design can use different strategies to implement different objectives and for that  we have synthesis exploration utility which shows how the design delay and area is effected by synthesis strategy. OpenLANE also has design exploration utility which can be used to sweep the design configurations and there are than 16 of them, and generate report of different design matrix and also show the number of violations generated after generating the layout. This is useful to find best coonfiguration for openLANE design.  
* After synthesis comes the testing structure i.e Design for test (DFT) insertion. If we want our design to be ready for testing we can enable this step which is optional. It uses opensource program *Fault*. It performs scan insertion, automatic test pattern generation, test patterns, fault coverage, fault simulation etc.
* Then comes the physical design implementation. It is called automated PnR ( Placement and Routing). It consists of several steps which is done by *Openroad* opeen source tool. These are floor/power planning, decoupling capacitor and tap cells insertion, placement, post placement optimisation, cts and routing, performed chronologically.
* As we perform optimization which involves some transformation of gate level netlist that was generated by the synthesis step. We need to perfomr Logic Equivalance Checking (LEC)
* and then this can be done by *yosys*. So we compare the netlist result from the optimization done during physical implementation to the gate level netlist formed during synthesis to make sure we are logically equivalance.
* During physical implementation we have a special step and this is antenna diode insertion script. This step is required to address the antenna rules violations. As when a wire segment is fabricated ans it is long enough it can act as an antenna. It collects charges which can damage transistor gate during fabrication. So legnth of the wire of these transistor gate must be limited usually this is the job of router.
* With openLANE we can take a preventive approach. In this we add a fake antenna diode next to every cell input after placement. This fake antenna diode is not real diode but follows the footprints of the library antenna diode. We run the antenna checker i.e *Magic* on the router layout and if the check reports violations on the cell input pin, replace the fake diode cell by a real one.
* The sign off of OpenLANE has steps like Static Timing Analysis (STA), Physical Verification, Design Rules Checking (DRC) and Layout vs Schematic (LVS). LVS is done by *Magic* and *Netgen*, extracted by SPICE by Magic vs Verilog netlist.

## DAY-1 
### Inception of opensource EDA tools, OpenLANE and Sky130 PDK
The day 1 started off with explanation of chip as a package and various MACROS inside the chip. Then we had a brief introduction to RISK-V Instruction Set Architecture (ISA) followed by detailed explanation on how software interacts with hardware. Later we were given introduction to all components of open-source digital ASIC design such as HDL, RTL, IP's of the function that we want to implement, EDA tools and process design kit. Then we were givem a simplified RTLIIGDS flow i.e synthesis,  floor and power planning, placement, clock tree synthesis, signal routing and static timing analysis. After that we were given introduction to openLANE and strive chipsets followed by detailed ASIC design flow. Then we had a tour of OpenLANE directory structure in detail.

#### Invoking openLANE in interactive mode

![](DAY1/op1.png)

![](DAY1/op2png)

#### Performing intial preprations for picorv32a

![](DAY1/op3.png)

#### Running synthesis

Use `run_synthesis` 

![](DAY1/op4.png)

once the synthesis is complete you get the slack report of min and max paths of clock

## DAY-2
### Good floorplan vs bad floorplan and introduction to library cells
The day 2 started of with the concepts of defining width and height of core and die followed by explanation of utilisation factor i.e *ratio of area occupied by netlist and total area of the core* as well as aspect ratio which ratio of *height and width*. Explanation on preplaced cell was given which are basically macros and IP's that are implemented once (i.e placed only once on the chip) and can be instantiated multiple number of times onto a netlist. This has to be done before routing. The locations of preplace cells are fixed. We also surroundthese cells with decoupling to avoid crosstalk.
Then the concept of powerplanning was explained where we learnt the concept of ground bounce and also why we use a mesh like structure ( multiple power supply) for pwer supply.
And later the concepts of pin placement was given.

During the lab sessions we were given the method to make runs folder with a custom folder name which actually saves all the activities using command :

`prep -design picorv32a -tag <file_name>`

We can calso overwrite flag using :

`prep -design picorv32a -tag <file_name> -overwrite`

![](DAY2/op5.png)

Before running the floorplan we observed the default floorplan config file, config.tcl and sky130A config file. The precedency of these files are **Sky130A config file> config.tcl> default floorplan config file**. Which means that any changes in **Sky130A config file** will overwrite the changes made in the other two. 
Then we fix the utilization factor, aspect ratio, the i/o pins and provide decap cells and welltap cells. To run the floorplan we use :

`run_floorplan`

![](DAY2/op6.png)

#### Envoke the Magic tool to view the layout by proving lef, tech library and def file.

![](DAY2/op7.png)

#### Layout



 











