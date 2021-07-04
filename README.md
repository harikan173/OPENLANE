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
![fp1](https://user-images.githubusercontent.com/84865915/124375288-24407380-dcbf-11eb-9b32-acdfb9c5a1e2.JPG)
![fp2](https://user-images.githubusercontent.com/84865915/124375295-2aceeb00-dcbf-11eb-88dc-a39b81822d2b.JPG)
![fp3](https://user-images.githubusercontent.com/84865915/124375308-315d6280-dcbf-11eb-8954-f16274627438.JPG)
![fp4](https://user-images.githubusercontent.com/84865915/124375313-38847080-dcbf-11eb-82fa-fce5719e9cb7.JPG)
![fp5](https://user-images.githubusercontent.com/84865915/124375322-3fab7e80-dcbf-11eb-83df-99327b842b2b.JPG)
![fp6](https://user-images.githubusercontent.com/84865915/124375326-45a15f80-dcbf-11eb-9564-04c83d597fd6.JPG)
![gitclone](https://user-images.githubusercontent.com/84865915/124375334-4b974080-dcbf-11eb-8b9f-df45f5effb0f.JPG)
![inverter](https://user-images.githubusercontent.com/84865915/124375340-50f48b00-dcbf-11eb-83d8-1a36fe9db7c6.JPG)
![p1](https://user-images.githubusercontent.com/84865915/124375358-623d9780-dcbf-11eb-9518-45a897031d72.JPG)


