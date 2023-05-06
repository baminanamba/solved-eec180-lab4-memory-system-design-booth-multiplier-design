Download Link: https://assignmentchef.com/product/solved-eec180-lab4-memory-system-design-booth-multiplier-design
<br>
<strong>Objective</strong>: In the first part of the lab, you will use Verilog to model RAM modules. In the second part, you will design a multiplier for signed binary numbers using Booth’s algorithm and verify using the testbench given to you.

<h1>Prelab</h1>

Read the lab carefully and show the steps associated with the multiplication of -10 and 9 using a Booth multiplier as shown in II.

<h1>I.       RAM</h1>

There are several ways to implement a memory component in an Intel FPGA. One method is to instantiate a RAM module from the Quartus Prime Parameterized Modules. The MegaWizard Plug-in Manager enables you to configure the RAM module to fit your desired specifications. Another way to implement a memory component is to model it behaviorally in Verilog. In this lab, you will implement memory components using behavioral modeling.

Memory can be specified in Verilog as a two-dimensional array. For example, a memory with 32 words and 4-bits per word can be declared with the statement:




reg [3:0] memory [31:0];




In the MAX 10 FPGA, memory can be implemented either using flip-flops or by using dedicated memory resources within the FPGA known as M9K blocks. Each M9K block contains 8192 memory bits that can be configured to implement various memory modules. There are 182 such blocks available on MAX10 chip. Depending on how you write your Verilog code, the Quartus Prime compiler will either infer flip-flops or M9K memory blocks to implement your memory device.




In this part, you will design <strong>two</strong> 16×4 RAM modules and implement them in the DE10 board. The block diagram for a 16×4 RAM is shown in Figure 1.







Figure 1. A 16×4 RAM module




You will test your memory modules using the switches, LEDs and 7-segment displays on the DE10 board. The I/O device assignments are as follows:







<table width="363">

 <tbody>

  <tr>

   <td width="264"><strong>I/O signal </strong></td>

   <td width="100"><strong>DE10 device </strong></td>

  </tr>

  <tr>

   <td width="264">Write Enable (for both RAM modules)</td>

   <td width="100">SW[9]</td>

  </tr>

  <tr>

   <td width="264">Select RAM module for writing</td>

   <td width="100">SW[8]</td>

  </tr>

  <tr>

   <td width="264">Clock</td>

   <td width="100">KEY[0]</td>

  </tr>

  <tr>

   <td width="264">Address (for both RAM modules)</td>

   <td width="100">SW[7:4]</td>

  </tr>

  <tr>

   <td width="264">Data_In (for both RAM modules)</td>

   <td width="100">SW[3:0]</td>

  </tr>

  <tr>

   <td width="264">Address Display</td>

   <td width="100">HEX3</td>

  </tr>

  <tr>

   <td width="264">Data_In Display</td>

   <td width="100">HEX2</td>

  </tr>

  <tr>

   <td width="264">RAM1 Data_Out Display</td>

   <td width="100">HEX1</td>

  </tr>

  <tr>

   <td width="264">RAM0 Data_Out Display</td>

   <td width="100">HEX0</td>

  </tr>

 </tbody>

</table>




Your memory modules must operate as follows:

<ul>

 <li>Only one RAM module (RAM1 or RAM0) can be written into at a time.</li>

 <li>The Select switch (SW[8]) determines which memory module will be written.</li>

 <li>A write operation will occur to the selected RAM module on a positive Clock transition when the Write Enable signal is active (high).</li>

 <li>Both RAM modules can be read to their respective 7-segment displays simultaneously.</li>

 <li>The RAM output data can be either clocked (synchronous) or unclocked (asynchronous).</li>

</ul>




Perform the following steps:




<ol>

 <li>Write the Verilog code to implement the two 16×4 RAM modules on the Altera DE10 board in M9K blocks. Use <em>synthesis ramstyle pragma</em> as shown in the link, (<u>https://www.intel.com/content/www/us/en/programmable/quartushelp/17.0/hdl/v log/vlog_file_dir_ram.htm</u>).</li>

</ol>

Compile your program. Verify that your code infers M9K memory blocks by examining the Compilation Report. Ideally, you should also determine how to infer flip-flops for your memory devices instead of M9K blocks. You can also go to Tools -&gt; Chip Planner to see mapping of the resources on actual chip and in this case, one of the M9K blocks (Yellow color) must be enabled as shown below.




Figure 2. Instantiation of M9K as shown on Chip Planner




<ol start="2">

 <li>Modify your Verilog design to specify the initial contents of your RAM modules. The easiest way to do this is by using an <strong>initial</strong> For example, you could use a for-loop within an <strong>initial</strong> block to initialize the RAM contents. Another option is to use a MIF (Memory Initialization File) to assign initial values. (See the Quartus Prime documentation,</li>

</ol>

<u>https://www.intel.com/content/dam/www/programmable/us/en/pdfs/literature/hb/ qts/archives/qts-qpp-5v1-17-0.pdf</u>

for information on both methods of memory initialization.)




In addition, the Intel® Quartus® Prime software offers several IP cores to implement memory modes. See the Quartus Prime documentation,  <u>https://www.intel.com/content/dam/www/programmable/us/en/pdfs/literature/ug/ ug_ram_rom.pdf</u> for more information.




The <strong>initial</strong> construct is probably more straightforward for our application.




<ol start="3">

 <li>Download and test your design. Demonstrate to your TA that you can read out the initial contents of your RAM modules and that you can write new values to the RAMs.</li>

</ol>




<h1>II.  Multiplication of Signed Numbers</h1>




In this part, you will use Verilog to design and simulate a multiplier for twos complement, signed binary numbers using Booth’s algorithm.

<em> </em>

<em>Booth’s algorithm works as follows, assuming each number is n bits including sign: Use a (n+1)-bit register for the accumulator (A) so the sign bit will not be lost if an overflow occurs. Also, use an (n+1)-bit register (B) to hold the multiplier and an n-bit register (C) to hold the multiplicand. </em>




<ol>

 <li>Clear A (the accumulator), load the multiplier into the upper n bits of B, clear B<sub>0</sub>, and load the multiplicand into C.</li>

 <li>Test the lower two bits of B (B<sub>1</sub>B<sub>0</sub>).</li>

</ol>

If B<sub>1</sub>B<sub>0</sub> = 01, then add C to A (C should be sign-extended to n+1 bits and added to A using an (n+1)-bit adder).

If B<sub>1</sub>B<sub>0</sub> = 10, then add the 2’s complement of C to A. If B<sub>1</sub>B<sub>0</sub> = 00 or 11, skip this step.

<ol start="3">

 <li>Shift A and B together right one place with sign extended.</li>

 <li>Repeat steps 2 and 3, n-1 more times.</li>

 <li>The product will be in A and B, except ignore B<sub>0</sub>.</li>

</ol>

<table width="496">

 <tbody>

  <tr>

   <td colspan="2" width="312"> Example for n=5: Multiply -9 by -13.</td>

   <td rowspan="2" width="60"> </td>

   <td rowspan="2" width="60"> </td>

   <td rowspan="2" width="64"> </td>

  </tr>

  <tr>

   <td width="240"> </td>

   <td width="72"> </td>

  </tr>

  <tr>

   <td width="240"><strong>#       Action </strong></td>

   <td width="72"><strong>A </strong></td>

   <td width="60"><strong>B </strong></td>

   <td width="60"><strong>B</strong><strong>1B</strong><strong>0 </strong></td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">1. Load registers.</td>

   <td width="72">000000</td>

   <td width="60">100110</td>

   <td width="60">10</td>

   <td width="64">C=10111</td>

  </tr>

  <tr>

   <td width="240">2. Add 2’s complement of C to A.</td>

   <td width="72"><u>001001</u></td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">  </td>

   <td width="72">001001</td>

   <td width="60">100110</td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">3. Shift A&amp;B.</td>

   <td width="72">000100</td>

   <td width="60">110011</td>

   <td width="60">11</td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">3. Shift A&amp;B.</td>

   <td width="72">000010</td>

   <td width="60">011001</td>

   <td width="60">01</td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">2. Add C to A.</td>

   <td width="72"><u>110111</u></td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">  </td>

   <td width="72">111001</td>

   <td width="60">011001</td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">3. Shift A&amp;B.</td>

   <td width="72">111100</td>

   <td width="60">101100</td>

   <td width="60">00</td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">3. Shift A&amp;B.</td>

   <td width="72">111110</td>

   <td width="60">010110</td>

   <td width="60">10</td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">2. Add 2’s comp of C to A.</td>

   <td width="72"><u>001001</u></td>

   <td width="60"> </td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">  </td>

   <td width="72">000111</td>

   <td width="60">010110</td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

  <tr>

   <td width="240">3. Shift A&amp;B.</td>

   <td width="72">000011</td>

   <td width="60">101011</td>

   <td width="60"> </td>

   <td width="64"> </td>

  </tr>

 </tbody>

</table>




The result is: 0001110101 = 117







The controller for the multiplier can be implemented as a Mealy finite state machine (FSM) with only three states as shown below in Figure 2.




(B1B0=01) / Add               (B1B0=00) !Done) / Shift, Inc Count

(B1B0=10) / Add Complement




Figure 3. State Diagram for Booth Multiplier







In the reset state, S1, the controller waits for the Start signal. When the controller receives the Start signal, it should generate the Load signal to load registers B and C, clear bit B<sub>0</sub> and register A, and go into state S2. Note that this is a synchronous state machine so all state transitions must be synchronized to the system clock. In state S2, the controller will do the following:

<ul>

 <li>If B<sub>1</sub>B<sub>0</sub> = 00 or 11, shift A and B. If the termination count has been reached, go to S1. Otherwise, increment the count and remain in S2.</li>

 <li>If B<sub>1</sub>B<sub>0</sub> = 01, add C to A and go to S3.</li>

 <li>If B<sub>1</sub>B<sub>0</sub> = 10, add the 2’s complement of C to A and go to S3.</li>

</ul>

In state S3, registers A and B are shifted. If the termination count has been reached, the controller will go to S1; otherwise it will increment the count and go back to S2.




You may have noticed that the FSM operations in each state do not exactly coincide with the algorithm description given earlier. However, if you carefully compare the flows of the FSM and the algorithm, you will see that the FSM accomplishes the same operations with fewer state transitions.




Perform the following steps:




<ol>

 <li>Write the Verilog code for an 8-bit Booth multiplier (n=8). Your Verilog module should have the following inputs and outputs:</li>

</ol>

Inputs: Clock, Resetn, Start, Multiplier (Mplier), Multiplicand (Mcand) Outputs: Done, Product

<ol start="2">

 <li>Simulate your Verilog design with a test bench using the following test cases. The numbers are in twos-complement format.</li>

</ol>




01100110 x 00110011

10100110 x 01100110

01101011 x 10001110

11001100 x 10011001

10000000 x 10000000

11111111 x 11111111

00000000 x 01010101

01111111 x 01111111




<em>The test bench code is provided to you at the end of this write-up. Study the code so that you understand how it works. </em>

<ol start="3">

 <li>Demonstrate your simulation to your TA and have him sign a verification sheet. Print your simulation waveforms for a single multiplication.</li>

 <li>Synthesize your multiplier circuit and verify that it compiles without errors.</li>

</ol>

<h1>III.  Lab Report</h1>

For your lab report, include the following:

<ul>

 <li>Lab Cover Sheet with signed TA verification for successful download of Part I, simulation of Part II, and synthesis of Part II.</li>

 <li>Complete Verilog source code for your Parts I and II.</li>

 <li>Simulation waveforms of your functional simulations.</li>

 <li>Resource report indicating how many FPGA resources were required for each the designs in Parts I and II.</li>

</ul>