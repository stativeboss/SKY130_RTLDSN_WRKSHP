# SKY130_RTLDSN_WRKSHP
![image](https://user-images.githubusercontent.com/14873110/165369130-3d7d0459-3793-4838-ba00-81f3ecd547ba.png)
## Course Overview :
This five-day workshop by VSD (https://www.vlsisystemdesign.com/) gives an insight into RTL design and synthesis through theory and labs. It briefly explains the following steps of chip design:
1. Digital design. 
2. Validate the design through functional simualations and writing testbenches. 
3. Logical synthesis of the validated design.
4. Gate-level simulation of the synthesized net-list.

The workshop uses SkyWater OpenSource Process Design Kit (or PDK in short). SkyWater PDK (https://github.com/google/skywater-pdk) is a Free and Open Source Silicon(or FOSS in short) PDK that is an output of the collaboration between Google and SkyWater Technology. It targets the SKY130 process node (180nm- 130nm hybrid technology). 

This repo aims to make a record of the labs performed while briefing on what is covered in each video of the worshop. Each header in the table of contents below indicates a superficial heading which would comprise of either one or a few video's gist.


## Table of Contents

- [Introduction to open-source Verilog simulator iverilog](#introduction-to-open-source-verilog-simulator-iverilog)
- [Labs using iverilog and gtkwave](#labs-using-iverilog-and-gtkwave)
- [Introduction to Yosys and Logic Synthesis](#introduction-to-yosys-and-logic-synthesis)
- [Labs using Yosys and SKY130 PDKs](#labs-using-yosys-and-sky130-pdks)
- [Introduction to timing.libs](#introduction-to-timing-libs)
- [Heirarchial vs Flat Synthesis](#heirarchial-vs-flat-synthesis)
- [Various flop coding styles and Optimisation](#various-flop-coding-styles-and-optimisation)
- [Introduction to optimisations](#introduction-to-optimisations)
- [Combinational logic optimisations](#combinational-logic-optimisations)
- [Sequential logic optimisations](#sequential-logic-optimisations)
- [Sequential optimisations for unused outputs](#sequential-optimisations-for-unused-outputs)
- [GLS, Synthesis-Simulation mismatch, Blocking and Non-blocking statements](#gls-synthesis-simulation-mismatch-blocking-and-non-blocking-statements)
- [Labs on Sythesis and Simulation mismatch](#labs-on-synthesis-and-simulation-mismatch)
- [Labs on Synthesis and Simulation mismatch for blocking statements](#labs-on-synthesis-and-simulation-mismatch-for-blocking-statements)
- [If case constructs](#if-case-constructs)
- [Labs on Incomplete If case](#labs-on-incomplete-if-case)
- [Labs on Incomplete Overlapping If case](#labs-on-incomplete-overlapping-if-case)
- [For loop and For generate](#for-loop-and-for-generate)
- [Labs on For loop and For generate](#labs-on-for-loop-and-for-generate)
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

**Video-1:** <br />
In this video it is explained as to how we go about synthesizing the _good_mux_ design using Yosys.<br />

Actually.... the right video sequence for this heading is video2 -> video3 -> video1. So I'll doucment in this sequence:

**Video-2:** <br />
This video explains what logic synthesis is and how we go about synthesizing logic.<br />

_Question_: What is RTL Design?
_Answer_: It is the behavioural representation of the given specification.<br />

So, we have a code (RTL) and we want physical implementation of it (Using NAND Gates etc.,). This mapping is done by _Synthesis_. <br />

Design(input)                 -> Decide which gates to use -> Make connections between these gates -> Create a Netlist based on these connections (output) <br />
and Frontend Library (input) <br />


_Question_: What is a library file (extension .lib)?<br />
_Answer_: It is a 'bucket' of logical gates. It'll have different variations of a same gate (slow, medium, fast, 2 input, 3 input etc.,).<br />

_Question_: Why do we need different flavours of gates?<br />

**Video-3:** <br />

_Answer_: To meet the timing requirements (primarily... power and area are other creteria).<br />

**Note** : Faster gates provide good processing power but would be more wide and hence occupy more space. They might also lead to hold-time violations.<br />
 Just a quick recap: Hold-time is the time required for the signal to stay stable by the time clock arrives. (I'm the signal waiting at rly station for the clock train). The guidance offered to the Synthesizer regarding hold-time and set-up time violations is called 'Constraints'. <br />
 
 **Video-1:** <br />
 
 A bit about Yosys from the abstract of its manual: <br />
 
![image](https://user-images.githubusercontent.com/14873110/166118344-a68e0c9a-ff42-4954-a767-9da1aea604cd.png)

- The design is given as input to Yosys using ```read_verilog``` command.
- The .lib is given as input using ```read_liberty``` command.
- The ouput netlist is generated using ```write_verilog``` command.

**Note** :Liberty format is an industry standard to describe library cells of a particular technology (https://vlsiuniverse.blogspot.com/2016/12/liberty-format-introduction.html). It's manual can be found at https://people.eecs.berkeley.edu/~alanmi/publications/other/liberty07_03.pdf.

_Question_: How to verify the synthesis?<br />
_Answer_: Earlier we verified the design by giving the design file and the TB as inputs to the iverilog and then observing the output (VCD file) using gtkwave. Now, we give the Netlist(instead of design file) and TB as inputs to iverilog and observe in gtkwave. The waveforms should be exactly same as what we got when we run using design file.<br />

----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Labs using Yosys and SKY130 PDKs

Whatever is discussed regarding synthesis, is implemented in this heading in three videos. 

**Video-1**

1. Yosys is invoked using ```yosys``` command. **NOTE**: Check the directory before invoking yosys. It SHOULD be verilog_files directory as all the my_lib files are here.

![image](https://user-images.githubusercontent.com/14873110/166119299-d8c80aa6-cf6c-45c3-9f35-59ec50d869ac.png)

2. Use ```read_liberty -lib``` command to read the library.

![image](https://user-images.githubusercontent.com/14873110/166120197-5660ea21-319e-4e81-8ee3-fd1eb8ec87d2.png)

3. Use ```read_verilog``` command to read the design file.

![image](https://user-images.githubusercontent.com/14873110/166120432-c13cf275-076b-4297-9177-920a9b381da3.png)

**Note**: Need to explore about AST representation and RTLIL representation later. Kunal's reply when one of the workshopian raised this query:
```
It's an intermediate representation (like proto RTL)

You can find all documentation here
http://eddiehung.github.io/yosys/d1/d01/structRTLIL_1_1Design.html
```
4. Use ```synth -top```. More about this in later part of document.

![image](https://user-images.githubusercontent.com/14873110/166120654-c3058e7a-f0fd-4ff6-84ba-f71dc07427b7.png)

**Note**: Later, explore all the data that pops up after this command.

Small bit of all that data is snapped and put below. It shows the final result:

![image](https://user-images.githubusercontent.com/14873110/166120712-ba54541d-a837-4699-bc3b-e2e2f17c6718.png)

5. Use ```abc -liberty```. Here, we need to specify the directory and be wary of the double underscore before 'tt' sky130_fd_sc_hd__tt025C_1v80.lib.

![image](https://user-images.githubusercontent.com/14873110/166121434-406d1bdd-8a10-4614-a14d-544307cf3728.png)

 **Note**: This command basically converts RTL into gates and decides what gates it has to link to. Here too explore the data. Result snippet below:
 
 ![image](https://user-images.githubusercontent.com/14873110/166121487-93289625-1ef8-4e4c-86a5-c15c0d96b15b.png)

```
What happens if we execute the above command without specifying a particular library (in our case sky130)? 
The ABC loads it's in-built library and executes the command.

courtesy: Sam Kambadur
```

6. Use ```show``` command to see the graphical version (graphviz file with .dot extension) of logic it has synthesized.

![image](https://user-images.githubusercontent.com/14873110/166121557-93ba836b-0ed3-4284-9f07-266dfe810c22.png)

**Note**: The instructor's graphical version (shown below) is different from the one in mine. Probably the library got updated with a mux_cell.

![image](https://user-images.githubusercontent.com/14873110/166121795-47417de8-62ef-4428-80ec-b3b7fdb35ed3.png)


**Video-2**

The instructor makes sense of the graphical version that he got.

![image](https://user-images.githubusercontent.com/14873110/166121874-51fef153-30af-4ac8-bcfc-0f66c6b9ef31.png)

**Video-3**

1. Use ```write_verilog``` to get the netlist.

![image](https://user-images.githubusercontent.com/14873110/166122678-f5ee841a-4dea-4777-8090-54d34c9743b0.png)

**Note**: We can give any name, but good_mux_netlist makes more sense.

2. The netlist can be viewed by using ```!gvim good_mux_netlist.v``` command line.

![image](https://user-images.githubusercontent.com/14873110/166122757-8a453517-59c8-44b5-a353-0412c6012c31.png)

3. We may choose to use ```write_verilog -noattr good_mux_netlist.v``` to see an easily readable version of netlist. The below pic shows code after executing !gvim command.

![image](https://user-images.githubusercontent.com/14873110/166122812-ca7da7e6-cca6-461d-b434-c22a35759362.png)

------------------------------------------------------------------------------------------------------------------------------------------------------
## Introduction to timing libs

This section has three videos

**Video-1**

This video covers what exactly .lib file contains. <br />
Use ```gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib``` to open the .lib file.<br />

PVT: Process, Votage, Temperature -> These are very critical in working of a design.

```
tt: Typical (as in slow, fast, typical)
025C: Temperature
```

**Video-2**

![image](https://user-images.githubusercontent.com/14873110/166125412-972120cf-fd33-4608-9e82-ae9d17eb237f.png)

From the highlighted line onwards, as we go down we can observe various parameters mentioned.

Also, the as discussed earlier, the .lib file contains information about variations of a same cell type. That can be observed below (a311o_2 and a311o_4 for example):

![image](https://user-images.githubusercontent.com/14873110/166125475-8be04936-aeaa-42a2-85bf-c3dc0a5d1ee7.png)

**Note** in the gvim window, type the follwing code and get the .v file of a2111o gate:
```
:sp ../my_lib/lib/sky130_fd_sc_hd__a2111o.behavioural.v
```
Instead, if we wish to see the .v file with power ports included, type
```
:sp ../my_lib/lib/sky130_fd_sc_hd__a2111o.behavioural.pp.v
```
The below snapshot is taken from the lecture (as I didn't want to tamper the .lib file). It is observed that the parameters such as power etc., are different for different varity of cells  even if they are all 2 input AND gates only.

![image](https://user-images.githubusercontent.com/14873110/166125647-1a13c18c-c45f-46e1-8238-534a50310753.png)

It was also observed by one of the workshopian that the attribute cell_footprint is same for all the variants. Below is the summary of the discussion I had with him.

```
The cell_footprint attribute is given the same to all cells during the abc phase.
After PnR, whichever footprint class needs to be assigned (as per layout specs) would be assigned to this cell_footprint attribute 
(by swapping with some other footprint class or not swapping) using the in_place_swap_mode attribute.
```
---------------------------------------------------------------------------------------------------------------------------------------------------------

## Heirarchial vs Flat Synthesis
Has two videos

**Video-1**

_Question_: What does synth -top do?

1. Use command ```gvim multiple_modules.v```

![image](https://user-images.githubusercontent.com/14873110/166139041-d1bb7018-0d2c-484e-944a-8fdb34b1519a.png)

2. Open yosys using ```yosys``` command.
3. Use ```read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib```
4. Use ```read_verilog multiple_modules.v```. 

**Note**: By mistake, I first executed the ```read_verilog``` command and then used ```read_liberty``` command. After that I used ```read_verilog``` again and the following error showed:

![image](https://user-images.githubusercontent.com/14873110/166139329-f40cb3b2-c0f7-4086-a234-86e7fa4379ef.png)

Later, I exited the yosys environment and re-entered the right sequence: ```read_liberty``` and then ```read_verilog```. Ponder why the wrong sequence won't work.

5. Use ```synth -top multiple_modules```.
6. Use ```abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib```.

**Note**: Using ```show``` command will generate an error here as the 'multiple_modules' file has two submodules. So we need to specify exactly to show multiple modules.<br />

![image](https://user-images.githubusercontent.com/14873110/166142748-12174e07-602f-4413-aa98-8e9daa70324d.png)

7. Use ```show multiple_modules```

![image](https://user-images.githubusercontent.com/14873110/166142786-aae0d706-cdec-47e2-910b-40bdb3750136.png)

**Notes** This is called the Hierarchial Design

8. Use ```write_verilog -noattr multiple_modules_hier.v``` and then use ```!gvim multiple_modules_hier.v```.

**Note**: The instructors library (probably an old one) implements OR gate as below:

![image](https://user-images.githubusercontent.com/14873110/166144456-6921033a-159a-4a7e-b18b-165f69b44b86.png)

Check that the input literals are negated and then given as inputs to a NAND gate to realise an OR gate. This could also have been done with a NOR gate followed by an inverter. So, why did the tool 'chose' to use an extra inveter?<br />

![image](https://user-images.githubusercontent.com/14873110/166144706-56624810-77c4-4f43-b9af-ec978766c7e8.png)

**Note**: My library used a different gate:

![image](https://user-images.githubusercontent.com/14873110/166144743-5d3bb509-06a3-4fab-9074-5899e7f7297c.png)

The details of this can be found at https://bit.ly/3F8tOUu

**Video-2**:

1. Use ```flatten```.
2. Use ```write_verilog -noattr multiple_modules_flat.v```.

![image](https://user-images.githubusercontent.com/14873110/166145497-16828e7f-be88-4d55-b019-ca3d07c39cb7.png)

**Note**: We don't see any hierarchies in this one (such as sub-module1 or sub-module2 etc.,). It's a single netlist.
3. Use ```exit```.
4. Now follow this: 

```
1. yosys
2. read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog multiple_modules.v
4. write_verilog -noattr multiple_modules.v
5. synth -top multiple_modules
6. flatten
7. show
```

![image](https://user-images.githubusercontent.com/14873110/166146963-1bf54644-a80a-4873-a75b-985fe7371fb5.png)

**Note**: that there are no sub-modules in this one unlike the hierarchial case.<br >


_Given multiple modules, what is the way to synthesize at sub-module level?_

```
1. exit
2. yosys
3. read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
4. read_verilog multiple_modules.v
5. synth -top sub_module1
6. abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```
![image](https://user-images.githubusercontent.com/14873110/166153299-902f84c7-1fb3-4d13-be0b-eaf1738ff88b.png)

_Question_: Why sub-module level syntyhesis?<br />
_Answer_: 
1. Let's say we have a sub-module that repeats multiple times in a module. So instead of synthesizing same sub-module multiple times, it is time saving to synthesize it once and replicate it multiple times.
2. If we give entire module to synthesize, the tool might get overloaded (and difficult for designer to debug the netlist too). So we divide the whole module into various sub-modules and synthesize these sub-modules seperately.

--------------------------------------------------------------------------------------------------------------------------------------------------------

## Various flop coding styles and Optimisation

There are six videos under this heading. It explains how to code a flip-flop (FF), different types of FF available, and different coding styles possible. All the FF are available under verilog_files. Open these files using gvim.

**Video-1**

_Question_: Why Flip-Flops?<br />
_Theory_: **Glitch by propagation delay**

![image](https://user-images.githubusercontent.com/14873110/166161221-8352a404-a82f-4983-a038-07b8c6cc1730.png)

It is evident from the above explanation that combinational circuits suffer from glitches. Combination of combinational circuits is even more prone. <br />
So we want an element to store the value, making it more stable and less prone to glitches. And that is done by a FF.

![image](https://user-images.githubusercontent.com/14873110/166161586-3191c0ac-a7cf-48c6-91fe-fcbc0539cc51.png)


In set-up1, the glitch gets propagated to the output. This doesn't happen in set-up2 because of the presence of D-FF (this gives input to output only at posedge clk).

We need to initialise these FF as without initialisation, the combinational ckt would yield a garbage value. Set/Reset are used to initialise a FF. These signals could be either synchronous (with clock) or asynchronous (independent of clock).<br />

**Video-2**

From the dff_asyncret_syncres.v file:

![image](https://user-images.githubusercontent.com/14873110/166162300-b6fceb7a-b592-4574-b5f6-b7a24f9742bd.png)


- Observe that we don't give synchronous signal as trigger when calling the always block.
- If we wish to have asynchronous (or synchronous) Set instead of reset, just assign the output a value of 1 instead of 0 in the if/else conditions.

![image](https://user-images.githubusercontent.com/14873110/166162950-9115eb50-f3ca-4964-b910-23f4ed2886a6.png)

**Video-3**:

We first analyse the D FF with asynchronous reset. Use the following commands:

```
1. iverilog dff_asyncres.v tb_dff_asyncres.v
2. ./a.out
3. gtkwave tb_dff_asyncres.vcd
```

**Note**: By mistake, gave the extension in line 3 as .v instead of .vcd and the tool asked to check whether it is a vcd file!

![image](https://user-images.githubusercontent.com/14873110/166163201-38b704fa-55c3-4356-8bec-edd0df1a0b88.png)

The right command gives the follwing waveform:

![image](https://user-images.githubusercontent.com/14873110/166163258-2a84b200-a5fc-4fc4-a8fb-c131fa3eedb9.png)

It may be observed that the output goes low as soon as async_reset goes high **independent** of the clock signal or D.

Next we analyse the D FF with synchronous and asynchronous reset. Use the following commands:

```
1. iverilog dff_asyncres_syncres.v tb_dff_asyncres_syncres.v
2. ./a.out
3. gtkwave tb_dff_asyncres_syncres.vcd
```

![image](https://user-images.githubusercontent.com/14873110/166163533-227e8004-93eb-45ee-85c1-bb296d6eab8d.png)

In the above snapshot, observe that when the clk is low the output is still high even if the sync_reset is high. As soon as the posedge clk is detected, the output falls to zero since the sync_reset is high.

![image](https://user-images.githubusercontent.com/14873110/166163629-10c19c43-ca70-4a8e-b4e6-2d3d25ae879b.png)

That is not the case in case of async_reset. As can be witnessed from the above pic, once async_reset is high, the output falls to zero irrespective of clk or D.

**Video-4**:

In this video, the above circuits are synthesized using yosys.<br />

First is asynchrnous reset DFF. Follow the below commands:

```
1. read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
2. read_verilog dff_asyncres.v
3. synth -top dff_asyncres
4. dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib /*Many times we'll have a seperate library for FF, but here it's all in my_lib only*/
5. abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. show
```

![image](https://user-images.githubusercontent.com/14873110/166164821-4ab5978c-6642-41e7-9cba-28841441563b.png)

Next is synchronous/Asynchrnous reset DFF. Follow same commands as above by replacing dff_asyncres with dff_asyncres_syncres.

![image](https://user-images.githubusercontent.com/14873110/166165299-ef861910-3a30-4f06-b4d5-8bc62806e2cd.png)

Checkout https://antmicro-skywater-pdk-docs.readthedocs.io/en/test-submodules-in-rtd/contents/libraries/sky130_fd_sc_hd/cells/lpflow_isobufsrc/README.html


**Video-5**

 _Question_: What happens when we try multiplying a 3-bit number by 2?<br />
 Use the following commands in the yosys environment:
 ```
 1. read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 2. read_verilog mult_2.v
 3. synth -top mul_2
 4. abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 5. show
 ```
 
 ![image](https://user-images.githubusercontent.com/14873110/166165826-f56c9606-2901-41bf-bdb3-e84434307c7f.png)

 ![image](https://user-images.githubusercontent.com/14873110/166165889-d9f9b6d2-9966-4688-8012-385ab3b8c0af.png)
 
 
 **Video-6**
 
 Multiplying a 3-bit number with 9. <br />
 Check out the below theory:
 
 ![image](https://user-images.githubusercontent.com/14873110/166166149-f1d541a1-28ea-4ab5-ac3a-5fc928bbaf9c.png)


 Use following commands
 ```
 1. read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 2. read_verilog mult_8.v
 3. synth -top mult8
 4. abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 5. show
 ```
 
 ![image](https://user-images.githubusercontent.com/14873110/166166197-c49c000d-439f-4d8b-a88f-f21b182b4cdc.png)

![image](https://user-images.githubusercontent.com/14873110/166166247-027fe972-71a0-4c54-941d-471e358798ec.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------

## Introduction to optimisations
 
 This section has three videos. The first video briefs about combinational optimisation techniques while the next two videos brief about sequential optimisations.
 
 **Video-1**:
 
 ![image](https://user-images.githubusercontent.com/14873110/166169559-ae6a64a8-7137-487c-9b83-e5fbf796d3fb.png)


![image](https://user-images.githubusercontent.com/14873110/166169803-e96cec16-ae76-4d45-acbe-fad34676c6a7.png)

This is called _Boolean Logic Optimisation_.

**Video-2**:

Sequential logic optimisation techniques could be braodly classified as basic techniques (constant propagation) and advanced techniques. The following are the advanced techniques: <br />
- State optimisation
- Retiming
- Sequential Logic Cloning (Floor Plan Aware synthesis)

![image](https://user-images.githubusercontent.com/14873110/166172200-bf751f21-69ce-4cb3-bd8c-521011dada88.png)

**Note**: If the same set-up as above were to be done for a FF with Set input instead of Reset, we'd be tempted to say that the output follows Set signal. But upon timing diagram, we'd realise that the Set can make the output high asynchronously, but can't make it low!


**Video-3**:

- State Optimisation: Minimizing the number of states to reduce the number of FF used.
- Cloning: Observe the following figure

![image](https://user-images.githubusercontent.com/14873110/166174283-ecc81714-fa4e-4014-b6ef-5864dbf0f9c9.png)

But how is this an _optimisation_?

- Retiming: Observe the following figure

![image](https://user-images.githubusercontent.com/14873110/166174598-076f5d2b-b510-41b8-9a8e-ac7fe5362c97.png)

-----------------------------------------------------------------------------------------------------------------------------------------------------

## Combinational Logic Optimisations

This heading has two videos under it.

**Video-1**:

We use 'opt' files. These files can be seen by using ```ls *opt*``` command.

![image](https://user-images.githubusercontent.com/14873110/166217532-fa5f2f4f-a04c-468d-ab84-676f5def433e.png)

The following snippet shows what's in the opt_check.v:

![image](https://user-images.githubusercontent.com/14873110/166218212-1b0a16d4-9f2d-49e9-b145-abc7835b47e7.png)

The following snippet shows what's in the opt_check2.v:

![image](https://user-images.githubusercontent.com/14873110/166218349-b7a57e41-2c2f-4d97-9e97-80936e66eddc.png)

So basically, opt_check is a mux that'd act as an And gate while opt_check2 is a mux that'd act as an OR gate.

Upon synthesis, we see that the tool actually uses AND gate and OR gate instead of MUX based implementation. Refer below pics.

![image](https://user-images.githubusercontent.com/14873110/166220794-be945d59-42c1-422c-b944-8d2d1d6c2837.png)

![image](https://user-images.githubusercontent.com/14873110/166221263-0ca2796c-eaba-4a7a-ab08-b21a20522a12.png)

**Note**: The instructor uses a command ```opt_clean -purge``` before implementing the abc, to make all the optimisations. Intrestingly, I haven't used that command and still got an optimised output! This is happening probably because yosys automatically does some basic optimisation even if we don't ask it to.

**Video-2**:

Testing the optimisation for opt_check3.v, opt_check4.v, and multiple_module_opt.v. Note that the designs having multiple modules should be flattened before optimisation (but why?).

Refer below snippets:

opt_check3.v below:

![image](https://user-images.githubusercontent.com/14873110/166225704-a2eef1d7-919d-4031-a1c6-b1a9974a0b9d.png)

opt_check4.v below:

![image](https://user-images.githubusercontent.com/14873110/166227072-618689fa-870c-4bad-9a96-9626a7a7cf7d.png)

multiple_module_opt.v below (flattened the hierarchy):

![image](https://user-images.githubusercontent.com/14873110/166230902-a5af13e2-05f6-4283-b1ef-f5a1953a6c98.png)

**Note**: Synthesizing the above design, without flattening it resulted in the same graphviz too. So, why exactly should we flatten before optimizing?

Implemented the multiple_module_opt without flattening and without opt and the graphviz file is as below:

![image](https://user-images.githubusercontent.com/14873110/166231864-cdb5e3d8-863f-465d-a554-f655aa4646e1.png)

--------------------------------------------------------------------------------------------------------------------------------------------------

## Sequential logic optimisations


**Video-1**:

The files dff_const1.v and dff_const2.v are used. Opening these files in gvim using ```dff_const1.v -o dff_const2.v``` will open both of them in a same window.

![image](https://user-images.githubusercontent.com/14873110/166242557-3a1482b6-cb33-4b17-8e5e-a9b7bac70c3b.png)

![image](https://user-images.githubusercontent.com/14873110/166243199-c0478b97-7232-48f9-b5fe-1cbc808b27e0.png)


**Video-2**:

![image](https://user-images.githubusercontent.com/14873110/166243857-4ee0f672-06fb-4798-9ba7-e6d5708b9804.png)

The below snippet is for dff_const3.v:

![image](https://user-images.githubusercontent.com/14873110/166248716-91e6252c-a711-40ef-b043-6ae47367c980.png)

**Video-3**:

After synthesizing, this is what is found:

![image](https://user-images.githubusercontent.com/14873110/166249212-ef03dfcc-5a7d-49b0-851e-abdbd6267a9c.png)

The below snippet shows dff_const4.v and dff_const5.v opened in gvim:

![image](https://user-images.githubusercontent.com/14873110/166253465-d568d30f-c644-4196-a071-20a63a500e5e.png)

The below snippet shows the synthesized dff_const4.v and dff_const5.v:

![image](https://user-images.githubusercontent.com/14873110/166255003-a083c294-6a4f-451f-8180-c95c91f30d90.png)

---------------------------------------------------------------------------------------------------------------------------------------------------------

## Sequential Optimisations for Unused Outputs


Let's say we are getting a 3-bit output from a module, while we use only one of these bits as a primary output. The remaining bits which are not being used would be optimised. For example, consider:

![image](https://user-images.githubusercontent.com/14873110/166269528-8bbbe618-4d87-4b7a-8bd5-ab97f158711d.png)


But the synthesizer actually gives the below output:

![image](https://user-images.githubusercontent.com/14873110/166270155-5e0299b8-e423-4899-8eef-105ff95d6367.png)

(The output is taken and inverted and fed as input to the same FF)

This is called _SEQUENTIAL OPTIMISATION FOR UNUSED OUTPUTS_

**Note**: Need to explore it further by changing the source file a little bit (changing output bit length, changing the initial state etc.,)

---------------------------------------------------------------------------------------------------------------------------------------------------------

## GLS, Synthesis Simulation Mismatch, Blocking and Non Blocking Statements

_Question_: What is GLS?<br />
_Answer_: Originally we ran simualtions with RTL code as unit under test. Now we'll run the simulation with netlist as unit under test. This is called Gate Level Simulation (or GLS in short).<br />

_Question_: Why GLS?<br />
_Answer_: We wish to verify whether the logic holds even after synthesis (reason why it might be different is answered next). Also, GLS considers the timing as we run it with _Delay annotation_ (https://www.linkedin.com/pulse/gate-level-simulation-comprehensive-overview-jerry-mcgoveran/).<br />

_Question_: Why does Synthesis-Simulation Mismatch happen?<br />
_Answer_: Mostly three reasons:<br />
          - Missing sensitivity list: Example..for a mux, let's say we ask the ouput to respond only for change in select (but not input change) <br />
          - Blocking/Non-blocking mishap: Let's say we have two FF, the output of which is assigned as input to the other. If we assign this first and then assign the             input of first FF to it's output, it'll create a mishap. _Use Non-blocking for sequential circuits_. <br />
          - Non-Standard Verilog coding<br /
Another instance of simulation-synthesis mismatch:
```
\snippet of full code\
always @(*)
begin
y = m&c;
m = a|b;
end
```
Observe that here, m is being used before it's value get's updated (previous value m is used). This seems like a register is being used, but in synthesis, y = c.(a+b).
This is a simulation synthesis mismatch. (Doubt: Simulation without optimisation and Synthesis with optimisation.. would that be a sim-sys mismatch?)

--------------------------------------------------------------------------------------------------------------------------------------------------------------

## Labs on Synthesis and Simulation Mismatch

The files ternary_operator_mux.v and its TB are given to iverilog and then the waveforms are viewed in gtkwave analyser:

![image](https://user-images.githubusercontent.com/14873110/166296132-5c5df889-adf9-4dd1-a98b-4bd694bb5c60.png)

The above waveform shows the behaviour of 2x1 mux.

Now we synthesize it and check:
![image](https://user-images.githubusercontent.com/14873110/166298053-9139e3d3-48bc-4d50-87f3-51e22a437d34.png)

Use the following commands:
```
1. iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux.v tb_ternary_operator_mux.v
2. ./a.out
3. gtkwave tb_ternary_operator_mux.vcd
```
![image](https://user-images.githubusercontent.com/14873110/166299013-810e709c-c06f-48ec-af02-f63c6fcec87b.png)

-------------------------------------------------------------------------------------------------------------------------------------------------------------

## Labs on Synthesis and Simulation mismatch for blocking statements

Blocking caveat implemented before GLS is shown below:

![image](https://user-images.githubusercontent.com/14873110/166299730-c9119f1c-f73f-4493-9271-489510f882e2.png)

The GLS is shown below:

![image](https://user-images.githubusercontent.com/14873110/166300291-31f72be6-c007-43de-a9cf-e99368238b4e.png)

Cross verifying with RTL gtkwave, we observe that the waveforms won't match.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

## If Case Constructs

If statement

- If esle statements are implemented as a series of muxes.
- Bad coding style leads to inferred latches (casued because of incomplete if statements).
- Inferred latches are good for sequentil circuit design but should not be there for combinational circuit design.<br />

Case statement

- Case statement is also going to infer a mux
- Incomplete case statement will also lead to inferred latches (for the undefined cases, the variables will latch onto output).
- Case statements with default case will avoid inferred latches.
- Partial assignments is another caveat that creates inferred latches. (Assign all the outputs in all the segments of case)

Note that if-else is executed on a priority manner. Meaning that if I have an if else statement, when the if condition is satisfied, the code comes out to end. This is not the same in 'case'. All the cases are checked one-by-one and whichever case satisfies is executed. That is why we should not have an overlapping case statement.

------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Labs on Incomplete If case

The file incomp_if.v is analysed:<br />

Input the following commands:
```
1. gvim *incomp* -o /this opens all the files that have the string 'incomp' in their file name/
2. iverilog incomp_if.v tb_incomp_if.v
3. ./a.out
4. gtkwave tb_incomp_if.vcd
```

![image](https://user-images.githubusercontent.com/14873110/166313639-705baeba-cf83-48db-86e4-24f1a7a0dafe.png)


![image](https://user-images.githubusercontent.com/14873110/166313249-d9fd4cf5-e640-41e0-9bb7-243c0510e70c.png)

Upon synthesis, it yields following graphviz:

![image](https://user-images.githubusercontent.com/14873110/166313838-3bc7b884-7481-4c3c-8f1f-be71d58f6c06.png)

The file incomp_if2.v is analysed:<br />

![image](https://user-images.githubusercontent.com/14873110/166314602-9ddcb1e9-50a9-40f4-bf8a-6ca0c17dfb60.png)

Upon simulation:

![image](https://user-images.githubusercontent.com/14873110/166315630-9a9028c0-ebae-4fd2-ab8d-da8eef49de54.png)

Upon synthesis:

![image](https://user-images.githubusercontent.com/14873110/166315732-d384ab79-9540-4a67-987d-fff88b95fed6.png)


-------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Labs on Incomplete Overlapping If case

Use ```gvim comp_case.v -o incomp_case.v -o partial_case_assign.v -o bad_case.v``` to open these files's script in gvim.

The following are the observations for incomp_case.v:

![image](https://user-images.githubusercontent.com/14873110/166326146-936d2f40-bd86-4fe7-99af-43df14c73627.png)

Upon simulation:

![image](https://user-images.githubusercontent.com/14873110/166326244-1bc804d2-ffb9-491a-8355-c33cacac9a2b.png)

Upon synthesis:

![image](https://user-images.githubusercontent.com/14873110/166326466-7bc03faa-3760-46a2-9d03-1d3fc33db896.png)

The following are the observations for comp_case.v:

![image](https://user-images.githubusercontent.com/14873110/166327580-fdce063f-b49c-4850-a339-3bcbad4ab824.png)

Upon simulation:

![image](https://user-images.githubusercontent.com/14873110/166327706-6b0329e3-b0f5-4b2e-88d2-b47b260f77ea.png)

Upon synthesis:

![image](https://user-images.githubusercontent.com/14873110/166327894-28d91599-0969-4c24-ba1b-4370b6b3556d.png)

The following is a partial assign case:

Consider the code snippet below with i0, i1, and i2 as inputs, and x, and y as ouputs:

```verilog
case(sel)
    2'b00 : begin
            y = i0;
            x = i2;
            end
    2'b01 : y = i1;
    default : begin
              x = i1;
              y =i2;
              end
 endcase
 ```
 In case 01, x is not defined. So, even if we have a default case, when the case is 01, x will infer a latch. 
 
 ![image](https://user-images.githubusercontent.com/14873110/166329825-668056a8-1762-4528-940a-462e55bea24e.png)

Observe that there are no latches in the path of y. There is one latch in x's path.

The following is a bad case:

Consider the code snippet below with i0, i1, and i2 as inputs, and y as ouput:

```verilog
case(sel)
    2'b00 : y = i0;
    2'b01 : y = i1;        
    2'b10 : y = i2;
    2'b1? : y = i1;
    
endcase
 ```
 
 This is a bad case as the tool will get confused whether to assign y as i1 or i2 when the MSB of select line is high.
             
-------------------------------------------------------------------------------------------------------------------------------------------------------------------


## For loop and For generate

Loop constructs are of two types: for loop (evaluating expressions) and generate for loop (instantiating hardware multiple times) <br />

For loops are used in 'always' block, while the Generate for loops are used outside of the always blocks.<br />

- For loop comes in handy in implementing very wide mux/de-mux
- We can also use if-generate block, but for-generate and if-generate should be used only outside the always block.

One example where for generate comes in handy would be a ripple carry adder where a full adder needs to be instantiated multiple times.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Labs on For loop and For generate



































 
 


 






















































 
 
 











   


 




  






