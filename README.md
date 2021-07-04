# OPENLANE
# OpenSource Physical Design
  This repository contains all the information studied and created during the [Advanced Physical Design Using OpenLANE / SKY130](https://www.vlsisystemdesign.com/advanced-physical-design-using-openlane-sky130/) workshop. It is primarily foucused on a complete RTL2GDS flow using the open-soucre flow named OpenLANE. [PICORV32A](https://github.com/cliffordwolf/picorv32) RISC-V core design is used for the purpose.

# Table of Contents
  - [Introduction To RTL to GDSII Flow](#introduction-to-rtl-to-gdsii-flow)
  - [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [Basic IC Design Terminologies](#basic-ic-design-terminologies)
    - [Introduction To RISC-V](#introduction-to-risc-v)
    - [SoC Design and OpenLANE](#soc-design-and-openlane)
      - [Open-Source PDK Directory Structure](#open-source-pdk-directory-structure)
      - [What is OpenLANE](#what-is-openlane)
    - [Open-Source EDA Tools](#open-source-eda-tools)
      - [OpenLANE Initialization](#openlane-initialization)
      - [Design Preparation](#design-preparation)
      - [Design Synthesis and Results](#design-synthesis-and-results)
  - [Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
    - [Chip Floorplanning](#chip-floorplanning)
      - [Utilization Factor and Aspect Ratio](#utilization-factor-and-aspect-ratio)
      - [Power Planning](#power-planning)
      - [Pin Placement](#pin-placement)
      - [Floorplan using OpenLANE](#floorplan-using-openlane)
      - [Review Floorplan Layout in Magic](#review-floorplan-layout-in-magic)
    - [Placement](#placement)
      - [Placement and Optimization](#placement-and-optimization)
      - [Placement using OpenLANE](#placement-using-openlane)
    - [Cell Design and Characterization Flows](#cell-design-and-characterization-flows)
      - [Cell Design Flow](#cell-design-flow)
      - [Characterization Flow](#characterization-flow)
  - [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
    - [CMOS Inverter Design using Magic](#cmos-inverter-design-using-magic)
    - [Extract SPICE Netlist from Standard Cell Layout](#extract-spice-netlist-from-standard-cell-layout)
    - [Transient Analysis using NGSPICE](#transient-analysis-using-ngspice)
  - [Day 4 - Pre-layout timing analysis and importance of good clock tree](#day-4---pre-layout-timing-analysis-and-importance-of-good-clock-tree)
    - [Magic Layout to Standard Cell LEF](#magic-layout-to-standard-cell-lef)
    - [Timing Analysis using OpenSTA](#timing-analysis-using-opensta)
    - [Clock Tree Synthesis using TritonCTS](#clock-tree-synthesis-using-tritoncts)
  - [Day 5 - Final steps for RTL2GDS](#day-5---final-steps-for-rtl2gds)
    - [Generation of Power Distribution Network](#generation-of-power-distribution-network)
    - [Routing using TritonRoute](#routing-using-tritonroute)
    - [SPEF File Generation](#spef-file-generation)
  - [References](#references)
  - [Acknowledgement](#acknowledgement)
# Introduction To RTL to GDSII Flow
  RTL to GDSII Flow refers to the all the steps involved in converting a logical Register Transfer Level(RTL) Design to a fabrication ready GDSII format. GDSII is a database file format which is an industry standard for data exchange of IC layout artwork.
  The RTL to GSDII flow consists of following steps:
  - RTL Synthesis
  - Static Timing Analysis(STA)
  - Design for Testability(DFT)
  - Floorplanning
  - Placement
  - Clock Tree Synthesis(CTS)
  - Routing
  - GDSII Streaming
# Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
 ## Basic IC Design Terminologies
  During the Physical Designing, one will come across multiple terminologies that are frequently used. Some of them are mentioned below:
  - Package: It is a case that surrounds the circuit material to protect it from physical damage or corrosion and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). The below snippet shows an IC with 48 pins and Quad Flat No-Leads(QFN) package.
  - Die: A die is a small block of semiconducting material on which a given functional circuit is fabricated.
  - Core: It is the actual area of the IC where the logic resides.
  - Pads: These are the interfaces between the internal signals of a chip and the external pins

![1aaa](https://user-images.githubusercontent.com/84865915/123954341-e8a76000-d9c5-11eb-9faa-39eb1fb6bf61.JPG)
 ## SoC Design and OpenLANE
 ### Open-Source PDK Directory Structure
   All the Process Design Kit(PDK) are listed under the `pdks/` directory. Along with the `Sky130A` we are using some other open-source PDKs and other related files are also available in the directory. The location of the PDK directory is given of `$PDK_ROOT` variable.    
  
 ### What is OpenLANE
   [OpenLANE](https://github.com/efabless/openlane) is an automated RTL to GDSII flow which includes various open-source components such as OpenROAD, Yosys, Magic, Fault, Netgen, SPEF-Extractor. It also facilitates to add custom design exploration and optimization scripts.
   The detailed diagram of the OpenLANE architecture is shown below:
   
 ![2a](https://user-images.githubusercontent.com/84865915/123954395-f826a900-d9c5-11eb-9240-cc218f1f2800.JPG)
   

![oo1](https://user-images.githubusercontent.com/84865915/123960406-e3014880-d9cc-11eb-9385-dbe01400d3ea.JPG)

There are two directories available 
- 1-openlane
- 2-pdk 
- pdk stands for (process design kit),and the pdk we are using is sky water 130nm pdk,and openlane is there on this pdk.

   
![002](https://user-images.githubusercontent.com/84865915/123960735-47240c80-d9cd-11eb-869f-ae5068bb86b6.JPG)

As we open the pdks directory it contains three directories inside it:
- skywater-pdk(It contains all the pdk related files like:
   - Timing libraries
   - Lib file
   - Tech file
- open_pdk
   - it is used to make foundary level pdks compatible with
- sky130A

![004](https://user-images.githubusercontent.com/84865915/123960681-35db0000-d9cd-11eb-9021-be36cb8c7383.JPG)
- open lane has close to 30 to 40 designs

![005](https://user-images.githubusercontent.com/84865915/123960636-26f44d80-d9cd-11eb-8993-c535ea4bf6c3.JPG)
- the above screenshot shows the different designs that are already built in the openlane
- when we enter the picorv32a folder it contains 3 folders
   - src
   - sky130A_sky130_fd_sc_hd_config.tcl
   - config.tcl
- src file contains the verilog file and as well as sdc information also present
- config.tcl it bypasses any configure that has been done in the openlane.and inside this we can see all the information about environment is set to picorv32a and also shows the   path from where the verilog files has been picked up and also shows the information about clock period
- When we are running the custom design then sky130A_sky130_fd_sc_hd_config.tcl this file it will not present, and even if we dont have sky130A_sky130_fd_sc_hd_config.tcl this file it wont effect the flow.

![006](https://user-images.githubusercontent.com/84865915/123960614-20fe6c80-d9cd-11eb-82e7-38e85b48a674.JPG)

     
 ## Open-Source EDA Tools
 ### OpenLANE Initialization
   For invoking OpenLANE in Linux Ubuntu, we should first run the docker everytime we use OpenLANE. This is done by using the following script:
    
    docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
   
   A custom shell script or commands can be generated to make the task simpler.
   
   - To invoke OpenLANE run the `./flow.tcl` script.
   - OpenLANE supports two modes of operation: interactive and autonomous.
   - To use interactive mode use `-interactive` flag with `./flow.tcl`


![003](https://user-images.githubusercontent.com/84865915/123960713-3f646800-d9cd-11eb-9af9-ee8e42e9b6ee.JPG)
### Design Preparation
   The first step after invoking OpenLANE is to import the openlane package of required version. This is done using following command. Here 0.9 is the required version of OpenLANE.
   
    package require openlane 0.9
       
- firstly we must import all the packages that are needed for this we must use the command package require openlane 0.9
- Next is the design set up stage 
   - commad used for set up is:prep -design design_name(eg:prep -design picorv32a)

![007](https://user-images.githubusercontent.com/84865915/123960599-1cd24f00-d9cd-11eb-9383-c4d2ea779594.JPG)

- merging the LEFs is happening(it is merging below both files
   - cell level LEF file
   - Technology level LEF file
 - after merging the files runs directory is created inside picorv32a and inside the runs directory todays date will be created and inside that date folder we have:
    - cmds.log
    - tmps
    - results
    - reports
    - logs
    - config.tcl
 - and inside the Results directory we have 
    - synthesis
    - placement
    - floorplan
    - routing
    - cts
    - magic
    - lvs
 - config.tcl shows the which all default parameters which is been taken by the run.
### Design Synthesis and Results
   The first step in OpenLANE flow is RTL Synthesis of the design loaded. This is done using the following command.
   
    run_synthesis
# flop ratio
![flop ratio](https://user-images.githubusercontent.com/84865915/123962077-b0584f80-d9ce-11eb-81a9-868dccdc9969.JPG)

- FLOP RATIO = Number of D flip flops / Total Number of cells
- from the above statistics we can see that 
   - number of d flipflops are = 1613
   - Total number of cells = 14876
   - FLOP RATIO = 1613 / 14876 = 0.01084 * 100 = 10.84
# aftersynth
![aftersynth](https://user-images.githubusercontent.com/84865915/123961958-899a1900-d9ce-11eb-9f02-327dbdbbbe6d.JPG)
- after the synthesis we can see all the mappings have been done
# checkingtiming
![checktiming](https://user-images.githubusercontent.com/84865915/123962001-961e7180-d9ce-11eb-9fe8-5bdd6ca4a51b.JPG)

# sta report

![sta report](https://user-images.githubusercontent.com/84865915/123962173-c8c86a00-d9ce-11eb-9109-8e27fe37ba1f.JPG)
# Day 2 - Good floorplan vs bad floorplan and introduction to library cells
## Chip Floorplanning
Chip Floorplanning is the arrangement of logical block, library cells, pins on silicon chip. It makes sure that every module has been assigned an appropriate area and aspect ratio, every pin of the module has connection with other modules or periphery of the chip and modules are arranged in a way such that it consumes lesser area on a chip.

## Utilization Factor and Aspect Ratio
Utilization Factor is ratio of the area of core used by standard cells to the total core area. The utilization factor is generally kept in the range of 0.5-0.7 i.e. 50% - 60%. Maintaining a proper utilization factor facilitates placement and routing optimization.

## Power Planning
Power planning is a step in which power grid network is created to distribute power to each part of the design equally. This step deals with the unwanted voltage drop and ground bounce. Steady state IR Drop is caused by the resistance of the metal wires comprising the power distribution network. By reducing the voltage difference between local power and ground, steady-state IR Drop reduces both the speed and noise immunity of the local cells and macros.

## Pin Placement
Pin placement is a important part of floorplanning as the timing delays and number of buffers required is dependent on the position of the pin. There are multiple pin placement option available such as equidistant placement, high-density placement.

## Floorplan using OpenLANE
Floorplanning in OpenLANE is done using the following command.

run_floorplan
![fp1](https://user-images.githubusercontent.com/84865915/124375288-24407380-dcbf-11eb-9b32-acdfb9c5a1e2.JPG)

## Review Floorplan Layout in Magic
Magic Layout Tool is used for visualizing the layout after floorplan. In order to view floorplan in Magic, following three files are required: 
- 1. Technology File (sky130A.tech) 
- 2. Merged LEF file (merged.lef) 
- 3. DEF File
![fp2](https://user-images.githubusercontent.com/84865915/124375295-2aceeb00-dcbf-11eb-88dc-a39b81822d2b.JPG)
![fp3](https://user-images.githubusercontent.com/84865915/124375308-315d6280-dcbf-11eb-8954-f16274627438.JPG)
![fp4](https://user-images.githubusercontent.com/84865915/124375313-38847080-dcbf-11eb-82fa-fce5719e9cb7.JPG)
![fp5](https://user-images.githubusercontent.com/84865915/124375322-3fab7e80-dcbf-11eb-83df-99327b842b2b.JPG)
![fp6](https://user-images.githubusercontent.com/84865915/124375326-45a15f80-dcbf-11eb-9564-04c83d597fd6.JPG)


## Placement
### Placement and Optimization
The next step after floorplanning is placement. Placement determines location of each of the components on the die. Placement does not just place the standard cells available in the synthesized netlist. It also optimizes the design, thereby removing any timing violations created due to the relative placement on die.

### Placement using OpenLANE
Placement in OpenLANE is done using the following command.

- run_placement
- The DEF file created during floorplan is used as an input to placement. Placement in OpenLANE occurs in two stages:

    - Global Placement
    - Detailed Placement
    - Placement is carried out as an iterative process till the value of overflow converges to 0.
  
 ![p1](https://user-images.githubusercontent.com/84865915/124375358-623d9780-dcbf-11eb-9518-45a897031d72.JPG)


# Day 3 - Design library cell using Magic Layout and ngspice characterization
Every Design is represented by equivalent cell design. All the standard cell designs are available in the Cell Library. A fully custom cell design that meets all rules can be added to the library. To begin with, a CMOS Inverter is designed in Magic Layout Tool and analysis is carried out using NGSPICE tool.

![gitclone](https://user-images.githubusercontent.com/84865915/124375334-4b974080-dcbf-11eb-8b9f-df45f5effb0f.JPG)

## CMOS Inverter Design using Magic
The inverter design is done using Magic Layout Tool. It takes the technology file as an input (sky130A.tech in this case). Magic tool provide a very easy to use interface to design various layers of the layout. It also has an in-built DRC check fetaure. The snippet below shows a layout for CMOS Inverter with and without design rule violations.

![inverter](https://user-images.githubusercontent.com/84865915/124375340-50f48b00-dcbf-11eb-83d8-1a36fe9db7c6.JPG)

## Extract SPICE Netlist from Standard Cell Layout
To simulate and verify the functionality of the standard cell layout designed, there is a need of SPICE netlist of a given layout. To mention in brief, "Simulation Program with Integrated Circuit Emphasis (SPICE)" is an industry standard design language for electronic circuitry. SPICE model very closely models the actual circuit behavior. Extraction of SPICE model for a given layout is done in two stages.

- Extract the circuit from the layout design.
     - extract all
- Convert the extracted circuit to SPICE model.
     - ext2spice cthresh 0 rthresh 0
     - ext2spice
- The extracted SPICE model like the first snippet shown below. Some modification are done to the SPICE netlist for the purpose of simulations, which is shown in the second snippet below.
![day4 1](https://user-images.githubusercontent.com/84865915/124377230-9b2e3a00-dcc8-11eb-9df4-2c6bc3f3371a.JPG)

## Transient Analysis using NGSPICE
The SPICE netlist generated in previous step is simulated using the NGSPICE tool. NGSPICE is an open-source mixed-level/mixed-signal electronic spice circuit simulator. The command used to invoke NGSPICE is shown below.

    - ngspice <name-of-SPICE-netlist-file>
- Following command is used to plot waveform in ngspice tool.

    - ngspice 1 -> plot Y vs time A
    
Below figure shows the waveform of Inverter output vs input w.r.t. time. Many timing parameters like rise time delay, fall time delay, propagation delay are calculated using this waveform
 # day4 3
 
# Day 4 - Pre-layout timing analysis and importance of good clock tree
In order to use a design of standard cell layout in OpenLANE RTL2GDS flow, it is converted to a standard cell LEF. LEF stands for Library Exchange Format. The entire design has to be analyzed for any timing violations after addition or change in the design.

## Magic Layout to Standard Cell LEF
Before creating the LEF file we require some details about the layers in the designs. This details are available in a tracks.info as shown below. It gives information about the offset and pitch of a track in a given layer both in horizontal and vertical direction. The track information is given in below mentioned format.

  - <layer-name> <X-or-Y> <track-offset> <track-pitch>


# Day 5 - Final steps for RTL2GDS
## Generation of Power Distribution Network
In a normal RTL to GDSII flow the generation of power distribution network is done before the placement step, but in the OpenLANE flow generation of PDN is carried out after the Clock Tree Synthesis(CTS). This step generates all the tracks, rails required for routing power to entire chip. Generation of power distribution network is done using following command.
- gen_pdn
# ![d5 1](https://user-images.githubusercontent.com/84865915/124377224-923d6880-dcc8-11eb-94d0-eb6c31e4f2ab.JPG)


## Routing using TritonRoute
OpenLANE uses TritonRoute, an open source router for modern industrial designs. The router consists of several main building blocks, including pin access analysis, track assignment, initial detailed routing, search and repair, and a DRC engine. The routing process is implemented in two stages:

    - Global Routing - Routing guides are generated for interconnects
    - Detailed Routing - Tracks are generated interatively. TritonRoute 14 ensures there are no DRC violations after routing.
The following command is used for routing.

 - run_routing
 # day5 2

## SPEF File Generation
Standard Parasitic Exchange Format (SPEF) is an IEEE standard for representing parasitic data of wires in a chip in ASCII format. Non-ideal wires have parasitic resistance and capacitance that are captured by SPEF. OpenLANE consists of a tool named, SPEF_EXTRACTOR for generation of SPEF file. It is a python based parser which takes the LEF and DEF files as input arguments and generates the SPEF file. The following command is used for invoking the SPEC_EXTRACTOR.

- cd <path-to-SPEF_EXTRACTOR-tool-directory>
- python3 main.py <path-to-LEF-file> <path-to-DEF-file-created-after-routing>

  
# References
- RISC-V: https://riscv.org/
- VLSI System Design: https://www.vlsisystemdesign.com/
# Acknowledgement
- Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd.
- Nickson Jose


