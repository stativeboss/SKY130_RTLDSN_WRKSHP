# SKY130_RTLDSN_WRKSHP
![image](https://user-images.githubusercontent.com/14873110/165369130-3d7d0459-3793-4838-ba00-81f3ecd547ba.png)
## Course Overview :
A good RTL design engineer is expected to understand which code (or code style) is synthesizeable and which is not. This five-day workshop by VSD (https://www.vlsisystemdesign.com/) gives an insight into this aspect through theory and labs. It briefly explains the following steps of chip design:
1. Digital design. 
2. Validate the design through functional simualations and writing testbenches. 
3. Logical synthesis of the validated design.
4. Gate-level simulation of the synthesized net-list.

The workshop uses SkyWater OpenSource Process Design Kit (or PDK in short). SkyWater PDK (https://github.com/google/skywater-pdk) is a Free and Open Source Silicon(or FOSS in short) PDK that is an output of the collaboration between Google and SkyWater Technology. It targets the SKY130 process node (180nm- 130nm hybrid technology). 

This repo aims to make a record of the labs performed while briefing on what is covered in each video of the worshop. Each header in the table of contents below indicates a superficial heading which would comprise of either one or a few video's gist. The final topic covers my findings while doing the assessments.


## Table of Contents

- [Introduction to open-source Verilog simulator iverilog](#introduction-to-open-source-verilog-simulator-iverilog)
- [Labs using iverilog and gtkwave](#labs-using-iverilog-and-gtkwave)
- [Introduction to Yosys and Logic Synthesis](#introduction-to-yosys-and-logic-synthesis)
- [Introduction to timing.libs](#introduction-to-timing.libs)
- [Heirarchial vs Flat Synthesis](#heirarchial-vs-flat-synthesis)
- [Various flop coding styles and Optimisation](#various-flop-coding-styles-and-optimisation)
- [Introduction to optimisations](#introduction-to-optimisation)
- [Combinational logic optimisations](#combinational-logic-optimisation)
- [Sequential logic optimisations](#sequential-logic-optimisation)
- [Sequential optimisations for unused outputs](#sequential-optimisation-for-unused-outputs)
- [GLS, Synthesis-Simulation mismatch, Blocking and Non-blocking statements](#gls-synthesis-simulation-mismatch-blocking-non-blocking-statements)
- [Labs on Sythesis and Simulation mismatch](#labs-on-synthesis-and-simulation-mismatch)
- [Labs on Synthesis and Simulation mismatch for blocking statements](#labs-on-synthesis-and-simulation-mismatch-forblocking-statements)
- [If case constructs](#if-case-constructs)
- [Labs on Incomplete If case](#labs-on-incomplete-if-case)
- [Labs on Incomplete Overlapping If case](#labs-on-incomplete-overlapping-if-case)
- [For loop and For generate](#for-loop-and-for-generate)
- [Labs on For loop and For generate](#labs-on-for-loop-and-for-generate)
- [Assesments](#assessments)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

## Introduction to open-source Verilog simulator iverilog

This is an introductory video to iverilog, Design, and testbenches. This video is best briefed in question-answer format.

Note: iverilog (or Icarus Verilog http://iverilog.icarus.com/) is a free (but copyrighted?) Verilog simulator and synthesis tool.

_Question_- What is a simulator?<br />
_Answer_- The design is done to meet a set of given specifications. **A simulator is a TOOL** used to check whether the final RTL design is matching in accordance with what we initially wanted to design.

_Question_- What is a testbench?<br />
_Answer_- **A testbench (or TB in short) is a code** that is used to provide stimulus to the RTL design and hence **verify** its functionality. This is done by generating something called **test_vectors**.

_Question_- How does a simulator work?<br />
_Answer_- A simulator keeps 'checking' for changes in the input and changes the output accordingly. **No change in the input => No change in the output**

_Question_- Explain the testbench set-up using a block diagram.<br />
_Answer_- ![image](https://user-images.githubusercontent.com/14873110/165384067-4e711427-2b11-4f42-9518-1811f38fa671.png)<br />
Note: The design may have primary input(s) and primary output(s), but a testbench doesn't.<br />

_Question_- What is gtkwave?<br /> 
_Answer_- The iverilog simulator takes in the design file and the testbench file as inputs and gives out a _vcd_ file (VCD: Value Change Dump, indicating that it changes with the change in input value). In order to view the vcd file, we use a tool called gtkwave. It is a GTK+ based waveform viewer (https://github.com/gtkwave/gtkwave).<br />

----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Labs using iverilog and gtkwave

There are three videos under this heading. 

**Video-1:**<br /> 
This is an introductoy video to the labs. This is **Lab-1** It explains about the tool flow and the file set-up required for other labs. The following steps are followed:

1. Open RemoteSpark and open the linux terminal.
2. Paste this code in the terminal: 
   ```
   git clone https://github.com/kunalg123/vsdflow.git
   ```
   //*This creates a clone of vsdflow. You should see the folder on your desktop*//

   The below sequence of codes will show the files that are present in the vsdflow folder:
   ![image](https://user-images.githubusercontent.com/14873110/165394205-ab3a3c77-0298-4c2d-952c-ba3c1a054dda.png)

3. In the vsdflow directory, clone another repository by using: 
   ```
   git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
   ```

   By using the ls command, we should be able to see the following folder in the vsdflow folder:
   ![image](https://user-images.githubusercontent.com/14873110/165395149-294027a6-a1f1-4ae6-877d-303c2fdbf243.png)

4. Check for the standard SKY130 library using the following sequence of commands:
   ![image](https://user-images.githubusercontent.com/14873110/165395883-fe31485b-f5d9-45cd-b67a-a4dc2403516a.png)

5. Come back to the sky130RTLDesignAndSynthesisWorkshop directory<br />
   note: use ```cd ..``` to come back one step out of the present directory, and get into verilog_files folder.<br />
   note: use ```cd foldername``` to enter into the _foldername_ directory.<br />
   Now use the _ls_ command to view all the verilog source files and testbench files required for the workshop labs. A screenshot is provided for reference:
   ![image](https://user-images.githubusercontent.com/14873110/165397441-75c6ab8e-e978-4495-b0e2-c7420b0d39b3.png)


**Video-2:**<br /> 
This is part-1 of the actual lab videos. This is **Lab-2 part-1**It talks on how to use iverilog and gtkwave. A 2x1 MUX is implemented (loaded).

1. A verilog file called good_mux.v(from the _verilog_files_ directory) is called along with its TB file tb_good_mux.v. The command used for this purpose is _iverilog    good_mux.v tb_good_mux.v_. A screenshot from the terminal is attached for reference:
   ![image](https://user-images.githubusercontent.com/14873110/165417277-2b312fbe-6985-4ce9-a4e7-9377daaf1ee4.png)

2. Once the above command is run, a new file called _a.out_ appears in the _verilog_files_ directory (refer below screenshot):
   ![image](https://user-images.githubusercontent.com/14873110/165418179-6f06f414-419f-4001-8771-9ae562f6b5ff.png)

3. The following command is used to execute this a.out file. 
   ```
   ./a.out
   ```
   This is going to create a dumpfile (refer below screenshot):
   ![image](https://user-images.githubusercontent.com/14873110/165418395-860b6131-0c13-4d4e-b3c2-12a8cb13f31f.png)

4. This VCD file is then run using gtkwave command as shown below:
   ![image](https://user-images.githubusercontent.com/14873110/165419523-5074d249-ead3-428b-bd5b-1d41a2805d77.png)

5. After the above step, a new window pops-up (GTKWave Analyzer). It is shown below:
   ![image](https://user-images.githubusercontent.com/14873110/165420862-323323ec-4e3c-403d-b6c6-8c463301c541.png)

6. If we observe the timescale in the above pic, each time step is 1ps (1ps = 10^-12s), but the simulation time is 300000ps (refer step4 pic last line). Because of      this, we can't observe the whole waveform until we zoom out. For this, click on this button (shown below):
   ![image](https://user-images.githubusercontent.com/14873110/165422388-6875ae9e-4859-4ccd-b880-4746e5c49c98.png)
   Then the waveforms should appear as shown:
   ![image](https://user-images.githubusercontent.com/14873110/165422433-a9d1c263-5b57-4622-ab1d-334aba2a2709.png)

   Check out the below figure to know what various other buttons do:
   ![image](https://user-images.githubusercontent.com/14873110/165422999-3337205b-80d4-4954-a5fa-8852afe03e13.png)
  
**Video-3:**<br />
This is part-2 of the actual lab videos. This is **Lab-3 part-2** In this video the file structure is analysed.

The following command is used to open the .v files:

```
gvim tb_good_mux.v -o good_mux.v
```
note: Check the directory before typing the above command. It should be _~/Desktop/vsdflow/sky130RTLDesignAndSynthesisWorkshop/verilog_files$_

The following window then opens:
   ![image](https://user-images.githubusercontent.com/14873110/165472993-c500fc17-abd2-41c1-8775-69c842883d2b.png)
   
The following is the TB (design code is completely seen in the above pic. Note that the design follows _behavioural_ style):
   ![image](https://user-images.githubusercontent.com/14873110/165473396-81221336-2939-4f5d-b8de-5c43b5203749.png)

 Note:
   ![image](https://user-images.githubusercontent.com/14873110/165477253-3342cc0f-7ff9-420e-b3d8-d6e73387f8ea.png)
 More notes:
   ![image](https://user-images.githubusercontent.com/14873110/165479227-966a8e9d-3c24-441b-8aa0-737d30150ae1.png)
 
-------------------
## Introduction to Yosys and Logic Synthesis

Firstly, if iVerilog can perform both simulation and synthesis (below pic taken from iverilog website http://iverilog.icarus.com/), why use Yosys?
   ![image](https://user-images.githubusercontent.com/14873110/165511483-0729576f-8bb0-4af0-8af9-654aef298763.png)

A co-workshopian(is this word even there?) asked same question and the TA replied:

   ![image](https://user-images.githubusercontent.com/14873110/165511840-f2647522-f8a3-4fb9-9731-4e6034c58720.png)
   
This section also have three videos.

**Video-1:**
In this video it is explained as to how we go about synthesizing the _good_mux_ design using Yosys.<br />

Actually.... the right video sequence for this heading is video2 -> video3 -> video1. So I'll doucment in this sequence:

**Video-2**
This video explains what logic synthesis is and how we go about synthesizing logic.<br />

_Question_: What is RTL Design?
_Answer_: It is the behavioural representation of the given specification.<br />

So, we have a code (RTL) and we want physical implementation of it (Using NAND Gates etc.,). This mapping is done by _Synthesis_. <br />

Design(input)                 -> Decide which gates to use -> Make connections between these gates -> Create a Netlist based on these connections (output) <br />
and Frontend Library (input) <br />


_Question_: What is a library file (extension .lib)?
_Answer_: It is a 'bucket' of logical gates. It'll have different variations of a same gate (slow, medium, fast, 2 input, 3 input etc.,).

_Question_: Why do we need different flavours of gates?
_Answer_: To meet the timing requirements (primarily... power and area are other creteria).

**Note** : Faster gates provide good processing power but would be more wide and hence occupy more space. They might also lead to hold-time violations.<br />
 Just a quick recap: Hold-time is the time required for the signal to stay stable by the time clock arrives. (I'm the signal waiting at rly station for the clock train). The guidance offered to the Synthesizer regarding hold-time and set-up time violations is called 'Constraints'. <br />
 
 
 











   


 




  






