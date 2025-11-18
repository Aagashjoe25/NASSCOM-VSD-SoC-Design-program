[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
![Logo](https://www.vlsisystemdesign.com/wp-content/uploads/2016/12/vsd_logo.jpg)


# Digital VLSI SoC Design and planning Training

Welcome to NASSCOM VSD digital SoC design workshop <br>
In this workshop I have learnt the RTL to GDS flow using openlane and use opensource tools such as magic and openSTA to view the def files and perform static timing analysis. I learnt how a verilog code is finally converted to gds and the steps involved using openlane

## OpenLane: Introduction to open source physical design flow

To design a chip, we need to know what are the three fundamental requirements for physical design of any chip

- HDL language <br>
 To describe the circuit as hardware level, first we need to describe the circuit in such a way understandable to computer and human as well,
  thus verilog is used to describe the circuit as behavioural level and many other languages such as VHDL and even C++ (HLS synthesis) and systemC is also used as HDL language
- EDA Tools <br>
 Now we have the language to describe the circuit but for the machine to understand we need software specially designed to process the verilog code and perform the RTL to GDS flow which can be done by specific tools for each flow <br>
  But these tools are developed by companies and requires license to use the tools for developing our chips such as Cadence and Synopsis, thus there is a need to make eda tools open source so that anyone with verilog knowledge can learn the process involved in physical design and can generate the gds file <br>
  Openlane is a open source project which uses other open source tools magic, openroad, openSTA, yosys and abc to implement a RTL to GDS flow completely from just verilog file, constraint file and required pdk files
- PDK Data <br>
 Now verilog and openlane is available but for the actual chip manufacturing i.e the fabrication process there should be libraries to each component used in verilog file such as pmos, nmos and basic gates <br>
  Thus the actual openlane process is now valid to synthesize, floorplan, placement and routing mapped to the libraries or lef and lib files as present in the pdk (process design kit)
  
## Introduction to OpenLANE Detailed ASIC Design Flow
OpenLane  started as an Open Source Flow for a true Open Source Tape-out experiment.It was from e-fabless.It is a platform which supports different tools such as Yosys,OpenRoad,Magic,KLlayout and some other Open source tools.It integrates the various steps of Silicon Implementation and abstracts it. At e-fabless they have an SOC family called Strive. Strive is a family of open everything SOCs having Open PDK, Open RTL, Open EDA.

<img width="670" height="475" alt="336509101-837ca897-9005-40c0-acda-a4adb68836bf" src="https://github.com/user-attachments/assets/fc5ebc81-48f0-4a47-b6ed-b03d49a7aa8f" />

# **RTL2GDS OpenLANE ASIC Flow Practical implementation**

# **Day 1 **

## 1. Understanding the Directories
<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-09-49" src="https://github.com/user-attachments/assets/455fd92d-f952-4e8a-b665-504b745a8b11" />
<br>

### `openlane/`
Contains the OpenLane application. This includes all the Docker and Tcl scripts required to open and run the OpenLane flow. You can access this environment using the Docker command.

### `pdks/`
Contains the technology and tool-specific library files used by OpenLane for the automated **RTL-to-GDS** flow.

---
<br>
<img width="1280" height="768" alt="Directory Structure 1" src="https://github.com/user-attachments/assets/3e2d1bae-8f09-4112-91b0-370a09db3ef4" />
<img width="1280" height="768" alt="Directory Structure 2" src="https://github.com/user-attachments/assets/c56ff0ef-82d3-4fca-83e2-c13f440a805a" />
<br>

### **`designs/`**
Located inside the `openlane` directory, this folder contains many open-source Verilog designs. 

* **Current Project:** We will be working on **`picorv32a`**, which is a RISC-V processor core.
---
## 2.Entering openlane and load picorv32a design
<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-15-16" src="https://github.com/user-attachments/assets/78c61ba7-f6a9-4b4c-82b7-7a5f857765fb" />
<br>

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
docker

```
- Type clear which makes the command prompt starts from first line and change to the openlane directory inside openlane_working_dir
and enter docker

---

<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-16-00" src="https://github.com/user-attachments/assets/d72d6d32-df9b-48ec-ab43-a97973aa35b3" />
<br>

```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
```
- run the openlane tcl script with interactive flag to enable running each step otherwise it completes till routing and load the openlane version and the design file which ensures the initialisation of environment variables and merges the lef files 
---

## 3.Steps to run synthesis

<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-45-09" src="https://github.com/user-attachments/assets/f5e45c42-67cc-433c-810b-7b7dba20c662" />
<br>

- Synthesis generates the netlist (synthesis.v file) by yosys tool which describes all connections between each modules at gate level and uses the merged lef file for mapping process done by abc tool
- Synthesis results:
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-45-28" src="https://github.com/user-attachments/assets/826be9cd-83ce-44d6-8db9-f80b7ebe04fe" />
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-45-57" src="https://github.com/user-attachments/assets/cd0e804a-243d-4b1d-8685-8c14935bd325" />
<br>

# **Day 2 **

## 1. Steps to run floorplan
<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-46-08" src="https://github.com/user-attachments/assets/514300cd-4b88-4b95-bab8-c5ec295a9e65" />
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-47-12" src="https://github.com/user-attachments/assets/886b1d29-1c5c-46ed-bac8-4b11d1e431a4" />
<br> <br>
```bash
run_floorplan
``` <br> <br>
- Floorplan generates the core and die area based on the netlist generated by the yosys and abc tool <br>
- Thus the total area and D flip flop to total chip area can be calculated as shown below
<br>
<img width="1267" height="696" alt="Screenshot 2025-11-18 193809" src="https://github.com/user-attachments/assets/39453008-f3ee-42c0-914a-9bb5391c5ab8" />
<br>

---
## 2.Navigate to the runs folder and view def file using magic
<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-49-58" src="https://github.com/user-attachments/assets/bff905c0-fb3d-489c-82e9-c8fea36afa83" />
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-50-27" src="https://github.com/user-attachments/assets/c1d6aceb-7820-419b-bb0d-e6356e3f71b9" />
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-50-51" src="https://github.com/user-attachments/assets/2adca5b4-a064-40fd-b381-bf8d174210f9" />

<br>

```bash
cd designs/picorv32a/runs/current_run/results/floorplan
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
- Open a new terminal by clicking the file in the top left corner of terminal and then new tab which opens a new terminal with same path
- The picorv32a contains the current run folder which openlane automatically creates when we load the design using prep -design command
- Inside runs folder has the current run with date and time has folder name and navigate inside it
- The results folder contains the generated files of each flow: synthesis, floorplan, placement, cts and routing
- Navigate to the floorplan and open the def file using magic
- In the `tkcon` window, enter the command "what"  to display cell details.
  -Generated picorv32 floorplan layout
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-54-44" src="https://github.com/user-attachments/assets/cede475f-404c-4474-bf4f-b6a2b53536af" />

---
## 3. Steps to run placement and view the placement
<br>
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-55-37" src="https://github.com/user-attachments/assets/73e6bb94-b176-4ad7-ba99-3e884dc287f8" />
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-56-16" src="https://github.com/user-attachments/assets/0e87ea0c-6d56-4c62-a5d8-b09f62c045ac" />

<br>
```bash
run_placement
```
- After floorplan only the input and output cells and decoupling capacitor are placed in the floorplan, in placement only the actual design cells i.e the gates are placed using the lef and lib files
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-56-37" src="https://github.com/user-attachments/assets/2f431c7d-cc13-4482-b979-87a146ea84b6" />
-Navigate to the same directory current_run/results/placement
- run the same magic command and just change the floorplan.def to placement.def at end
- View the generated placement layout in magic
<img width="1280" height="768" alt="Screenshot from 2025-11-18 19-57-56" src="https://github.com/user-attachments/assets/2cbcaa7d-28bb-4bb2-a8dc-fe3473e700db" />



## Day 3


# Inverter Characterization using Sky130 Model Files


# Day 3 labs



Magic layout view to cmos inverter
To get the cell files refer  
 [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign) 

To extract the parasitics and characterize the cell design use below commands in tkcon window.

    extract all
    ext2spice cthresh 0 rthresh 0
    ext2spice

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/3071b576-84ac-420c-9ad8-9c8afac1f35f)

Modify the file according to 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/d6f08b62-ae42-4283-abd4-23e9dbef5637)

Now the next step is to run the SPICE file in ngspice tool by using command ngspice sky130_inv.spice

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/9a095bc2-e916-4821-ba31-e48643c4ba97)



# Inverter Characterization using Sky130 Model Files

In this lab, we will characterize the inverter using ngspice and Sky130 model files. The goal is to extract key parameters from the simulation results.

## Parameters to Characterize

1. **Rise Time:**
   - The time taken for the output waveform to transition from 20% to 80% of its maximum value.
   - Using data points:
     - x0 = 6.16138e-09, y0 = 0.660007
     - x1 = 6.20366e-09, y1 = 2.64009
   - Rise time = x1 - x0 = 0.0422 ns

2. **Fall Time:**
   - The time taken for the output waveform to transition from 80% to 20% of its maximum value.
   - Using data points:
     - x0 = 8.04034e-09, y0 = 2.64003
     - x1 = 8.06818e-09, y1 = 0.659993
   - Fall time = x1 - x0 = 0.0278 ns

3. **Propagation Delay:**
   - The time taken for a 50% transition at the output (0 to 1) corresponding to a 50% transition at the input (1 to 0).
   - Using data points:
     - x0 = 2.18449e-09, y0 = 1.64994
     - x1 = 2.15e-09, y1 = 1.65011
   - Propagation delay = x1 - x0 = 0.034 ns

4. **Cell Fall Delay:**
   - The time taken for a 50% transition at the output (1 to 0) corresponding to a 50% transition at the input (0 to 1).
   - Using data points:
     - x0 = 4.05432e-09, y0 = 1.65
     - x1 = 4.05001e-09, y1 = 1.65
   - Cell fall delay = x1 - x0 = 0.0043 ns

## LEF File Creation
Now that we have successfully characterized the inverter, the next step is to create a LEF (Library Exchange Format) file.


wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz



**VLSI Layout Geometries and DRC Errors**

In this section, we explore independent example layout geometries (M3.1, M3.2, M3.5, and M3.6) and highlight the specific DRC (Design Rule Check) errors associated with each:

1. **M3.1 (Metal Width DRC):**
   - Violation: The metal trace width in M3.1 is below the specified minimum width threshold.
   - Error: Metal width does not meet design rules.

2. **M3.2 (Metal Spacing DRC):**
   - Violation: The distance between adjacent metal traces in M3.2 does not meet the required spacing.
   - Error: Metal spacing violation.

3. **M3.5 (Via Overlapping DRC):**
   - Violation: The vias in M3.5 overlap with each other.
   - Error: Via overlapping issue.

4. **M3.6 (Minimum Area DRC):**
   - Violation: The enclosed area within M3.6 does not meet the specified minimum area requirement.
   - Error: Minimum area violation.

Use the command `magic -d XR` to open the Magic tool

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/314d5337-9add-4e42-8f0c-7bd31439829b)

# Using Magic Tool: Filling an Area with Metal 3 and Creating a VIA2 Mask

In this guide, we'll demonstrate how to fill a selected area with metal 3 and create a VIA2 mask using the Magic layout tool.

## Steps:

1. **Select an Area and Fill with Metal 3:**
   - Open the Magic GUI.
   - Select the desired area on your layout.
   - Guide the pointer to the metal 3 layer.
   - Press `P` to fill the selected region with metal 3.

2. **Create the VIA2 Mask:**
   - Open the `tkcon` terminal within Magic.
   - Type the command: `cif see VIA2`.
   - The metal 3-filled area will now be associated with the VIA2 mask.


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/7068a406-8138-4245-aa7e-0b99659f74b9)

## **Lab exercise to fix Poly-9 error in Sky130 tech file**


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/4010493f-00a7-4adc-9659-931f505036ba)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/d06ec717-0daa-426c-a74d-badb8432d20e)

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/71f3aeac-5094-4ba9-97dd-25af197fe039)


Commands to run in tkcon window

     # Loading updated tech file
       tech load sky130A.tech

     # Must re-run drc check to see updated drc errors
      drc check

     # Selecting region displaying the new errors and getting the error messages 
      drc why


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/06869006-06a0-4e9a-a81a-3a9c1e70ab49)

Incorrectly implemented difftap.2 simple rule correction


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/b33b6e9b-2bbd-489e-a345-7baa6f243850)

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/d6cd4fb0-ef01-4c4d-83e7-0d46649e778f)


Insert new commands  in sky130A.tech file to update drc

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/4e68ee9e-baf0-4653-a3ae-38e85e3f9245)
 
 Screenshot of Fixed DRC 


 ![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/e91a3f3c-42c6-4383-b51c-f5fe3bd82e74)


## DRC error as geometrical construct

N well rules 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/2ee6e495-203d-415a-bc13-5a817b401abb)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/19362396-33bc-4d03-88c3-783aa85dfd37)

New commands inserted in sky130A.tech file to update drc

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/5c21a6ef-6fa4-4a52-b835-f1cde0c1ee34)

Updated Tech file 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/681097ff-f036-43dc-9851-b2809a9a64d2)

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/7d6951b0-5ae4-46fe-a02b-724e555fa700)

# Day 4 : Pre-layout timing analysis and importance of good clock tree 

# Timing Modeling Using Delay Tables and Converting Grid Info to Track Info

In this section, we'll explore timing modeling using delay tables and the process of converting grid information to track information. Let's break it down step by step:

## Timing Modeling with Delay Tables

1. **Delay Tables:**
   - Delay tables provide information about the delay (propagation time) of signals through various components (such as gates, wires, and interconnects).
   - These tables help estimate signal arrival times and ensure proper timing in the design.

2. **Usage:**
   - During the physical design process, delay tables are used to model the behavior of standard cells, macros, and other components.
   - They guide the placement and routing tools to optimize signal paths for timing closure.

## Converting Grid Info to Track Info

1. **Purpose:**
   - In physical design, we need to convert grid information (such as rows and columns) into track information.
   - Tracks represent predefined horizontal and vertical paths on each metal layer.

2. **Considerations:**
   - When designing standard cells, keep the following in mind:
     - Input and output ports should align with the intersection of vertical and horizontal tracks.
     - The standard cell's width should be an odd multiple of the track pitch, and its height should be an odd multiple of the vertical track pitch.

3. **LEF File Extraction:**
   - To proceed further, we require the LEF (Library Exchange Format) file for the Inverter cell.
   - Extract it from the current Inverter cell to provide essential information for the place-and-route (PNR) process.

4. **Understanding Tracks:**
   - Open the `tracks.info` file to learn more about the horizontal and vertical tracks available on each metal layer.
   - This file specifies pitch, spacing, and other relevant details necessary for efficient routing.




# Day 4 Labs

 Commands to open the custom inverter layout
![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/d84fc4fa-1e3c-4e42-8e1a-c55f8484730f)

    # Change directory to vsdstdcelldesign
    cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

    # Command to open custom inverter layout in magic
    magic -T sky130A.tech sky130_inv.mag &

 Commands for tkcon window to set grid as tracks of locali layer

    # Get syntax for grid command
    help grid

    # Set grid values accordingly
    grid 0.46um 0.34um 0.23um 0.17um

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/b179114d-78e3-4262-82a9-5401d638b274)
 

Condition 1 Verified 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/8664d63c-c5e6-4bdd-ad40-18ca5212cf2a)


Condition 2 verified

Horizontal Pitch 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/b2bdf188-7051-491d-85f4-b5db4a1ac66c)

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/c80d9f83-1670-4832-90b7-f4a228e6f717)

Condition 3 verified

Vertical Pitch

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/e53b2ba1-bd70-43f1-82c8-7d7bab7bd130)


 **Save the finalized layout with custom name and open it.**

    # Command to save as
    save sky130_vsdinv.mag

Saved Layout 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/f1bd0ad3-adcd-45f9-898e-b887531e0d47)


Generate lef from the layout.

Command for tkcon window to write lef

    # lef command
    lef write

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/9a19eb26-fdbb-43bb-b7ce-199410084d31)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/4c7a07ed-f74b-46fe-8e25-60d3a1b70f08)

Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/b29da277-c636-43c1-9e09-c74be8bddbe1)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/a6b4c67d-68b5-45b7-918f-6d2e8dba1b10)

     set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

     set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
     set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib" 

     set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

     set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

**Run openlane flow synthesis with newly inserted custom inverter cell.**

run_synthesis 


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/ab528845-f395-403e-b9fa-8e8dfefd275f)

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/563b32d8-8f8f-4a1d-8d0b-b905430e963a)

Commands to view and change parameters to improve timing and run synthesis
```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

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

After running above commands 

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/39023a0c-44ae-42f3-813d-52756bfb7ccb)


Now run Floor plan

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/ce82bab8-b074-40c1-893e-a94cc78288d2)

```
# Now we can run floorplan
run_floorplan
```

but it failed 

So, we use 

```
init_floorplan
place_io
tap_decap_or
```

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/6d6a5b21-2b06-4b7a-b513-e813d8e7e715)


```

# Now we are ready to run placement
run_placement

```

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/fd4dc6a2-f937-4fbd-8d52-67b4964a9a27)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/b7c8cdb1-5b2a-4216-b99e-119553f2bf75)


```
# Command to view internal connectivity layers
expand
```

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/2c8de73a-0088-4ec1-b5f5-8afd2bbbcbdd)


Do Post-Synthesis timing analysis with OpenSTA tool.

Newly created pre_sta.conf for STA analysis in openlane directory

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/0bc39a88-65cd-4d83-abf5-4d60415658c9)

my_base.sdc



![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/0b5679e3-85f4-409c-b4da-b456e8c83258)

```
%run_synthesis
```

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/e9430c3a-8788-460e-accd-b49025af742e)


To fix this slack we use

```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

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

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/0fd53d84-a89d-4723-a0a4-72c270b4c6db)

Finally slack is met 



**Now run Placement**

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/b821c4da-f13b-4525-a149-ea6ae8682e4e)




# Day 5 GDS II Final step

## Power Distribution Network (PDN) Generation in OpenLANE

In OpenLANE, the PDN (Power Distribution Network) is crucial for proper power delivery within the chip. Let's walk through the steps to build the PDN:

## 1. PDN Generation

- The PDN ensures that all standard cells and macros receive adequate power.
- It provides a network of power rails (VDD and VSS) across the chip.

## 2. Using `gen_pdn` Procedure

- The `gen_pdn` procedure is responsible for running the PDN generation process.
- It sets up the power grid, defines power rails, and ensures proper connectivity.

## Common Issues

1. **`LIB_SYNTH_COMPLETE`:**
   - This variable must be defined in the `config.tcl` file.
   - It is called by the `gen_pdn` procedure defined in the `or_pdn.tcl` file.

2. **`LEF_MERGED_UNPADDED`:**
   - Ensure that this variable is correctly set in the `config.tcl` file.
   - It provides essential information for PDN generation.

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/47791054-ae49-4fb6-a8e0-cfa71befca92)

##Routing

Command to perform routing
```
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```
![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/22954699-23aa-461c-9dd9-68d04917fc53)

Power Planning :

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/132c579c-3522-4bde-8c1f-cdacf9984466)


Routing :

Default switches set for routing are :

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/37227eee-70bc-4cb8-9049-1178e9d7f210)


## VLSI Routing: Global Route and Detail Route

In VLSI design, the routing process is divided into two main stages: global route (or fast route) and detail route. Let's explore each stage:

## 1. Global Route (Fast Route)

- **Purpose:**
  - The global route focuses on quickly establishing a high-level routing solution.
  - It divides the chip into rectangular grid cells, forming a 3D routing graph.

- **Key Points:**
  - The global route creates a routing guide, which consists of boxes (representing pins of cells).
  - The output of the global route is a set of routing guides for each net.

## 2. Detail Route

- **Performed by Engine:**
  - Detail route is executed by the engine called tritonRoute.
  - It refines the routing solution obtained from the global route.

- **Using Global Route Output:**
  - Detail route utilizes the output from the global route, including the routing guides.
  - Algorithms are applied to find the best possible connectivity among all the routing points.



![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/7050b078-695e-4800-82df-6517d0a8c290)



![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/bedc2559-2ea8-43cb-9bd0-67d267ee4cd5)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/512e44d9-66fc-4f62-abe6-3374e787f5a0)


![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/1a773bfa-f827-4cfc-bff9-bd10a44900c0)

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/6ebd7dc1-e06d-4c03-9427-2947c15b1a67)
##  Post routing STA
Hold Slack

![image](https://github.com/user-attachments/assets/d723aa59-546b-4bc2-bce5-d59eef84a388)
Setup Slack
![image](https://github.com/user-attachments/assets/d8145ffc-3f21-4449-b7c0-bb27c241cf49)

## Certificate of Completion

![image](https://github.com/AnoushkaTripathi/NASSCOM-VSD-SoC-design-Program/assets/98522737/094094c7-9bcf-422c-8466-2e94da28d331)



## 🔗 Links
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://663d078622096c25eed3ceff--lighthearted-pasca-dc5929.netlify.app/)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/anoushkastripathi/)
