## The is the note I took during the Cadence online training: Genus Synthesis Solution with Stylus Common UI v25.1

## Basic Synthesis Flow
- Set technology library and inital setup
  - ```read_libs``` 
  - Use ```read_mmmc``` if running MMMC flow
- Read HDL files
  - ```read_hdl ${RTL_LIST}```
- Elaborate the design
  - ```elaborate```
- Initialise the design
  - ```init_design```
  - Optional: Only required if the MMMC file is read
- Set timing and design constraints
  - ```read_sdc```
  - If running MMMC flow, constraints are specified in the MMC file
- Apply optimisation directives
- ```syn_generic <-physical>```
- ```sun_map <-physical>```
- ```syn_opt <-spatial>```
- Report and analysis
- Write output files
- Constrints meet?
  - Yes: Go the P&R
  - No: Go back to **Apply optimisation directives**, and modify the optimisation directives

## Applying OPtimisation Directives

* Preserving instances and subdesigns
  ```Tcl
  set_db <module/inst> .dont_touch {false | true | const_prop_delete_ok | const_prop_size_deleete_ok | delete_ok | map_size_ok | size_ok | size_delete_ok}
  ```
  Used on mapped moudle/instance to prevent any logic changes in the block whilst allowing mapping optimisations to be performed in surrounding logic.

  Note that, when the ```const_porp_size_delete_ok``` value is used, the tool is allowed to delete this instance as a reuslt of constant propagation through it.

  ||delete|resize|rename|remap|Propagate constant through|
  |:--|:--:|:--:|:--:|:--:|:--:|
  |```true```||||||
  |```false```|v|v|v|v|v|
  |```delete_ok```|v|||||
  |```const_prop_delete_ok```|v||||v|
  |```const_prop_size_delete_ok```|v|v|||v|
  |```size_ok```||v||||
  |```map_size_ok```||v|v|
  |```size_delete_ok```|v|v||||

  * Example 1
    To allows resizing, mappng and remapping of a sequential instance during the optimisation step, but not renaming or deleting it
    ```Tcl
    set_db inst:dut_top/vreg_q .dont_touch map_size_ok
    ```
  * Example 2
    To enable Genus to preserve the instance but alos to give the flexibility to size it
    ```Tcl
    set_db inst:dut_top/sram_apb_bridge_inst .dont_touch size_ok
    ```
## Grouping and Ungrouping of the hierarchy
T.B.D.

## Reporting ungroup modules
T.B.D.
