# OPENLANE
# oo1
There are two directories available 
- 1-openlane
- 2-pdk 
- pdk stands for (process design kit),and the pdk we are using is sky water 130nm pdk,and openlane is there on this pdk.
# 002
As we open the pdks directory it contains three directories inside it:
- skywater-pdk(It contains all the pdk related files like:
   - Timing libraries
   - Lib file
   - Tech file
- open_pdk
   - it is used to make foundary level pdks compatible with
- sky130A

# 004 
- open lane has close to 30 to 40 designs
# 005
- the above screenshot shows the different designs that are already built in the openlane
- when we enter the picorv32a folder it contains 3 folders
   - src
   - sky130A_sky130_fd_sc_hd_config.tcl
   - config.tcl
- src file contains the verilog file and as well as sdc information also present
- config.tcl it bypasses any configure that has been done in the openlane.and inside this we can see all the information about environment is set to picorv32a and also shows the   path from where the verilog files has been picked up and also shows the information about clock period
- When we are running the custom design then sky130A_sky130_fd_sc_hd_config.tcl this file it will not present, and even if we dont have sky130A_sky130_fd_sc_hd_config.tcl this file it wont effect the flow.
# 003

- firstly we must import all the packages that are needed for this we must use the command package require openlane 0.9
- Next is the design set up stage 
   - commad used for set up is:prep -design design_name(eg:prep -design picorv32a)
# 007
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
# 008 run synthesis
# flop ratio
# aftersynth
- after the synthesis we can see all the mappings have been done
# checkingtiming
report
# sta report
# actual synthesis statistic report
if we 
