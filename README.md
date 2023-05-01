Download Link: https://assignmentchef.com/product/solved-caa-homework-4
<br>
Section 1. Handwritten

<ul>

 <li>28 (12%)</li>

 <li>29 (12%)</li>

</ul>

–

Section 2. Programming

<ul>

 <li>Pipelined CPU (74%)</li>

 <li>Report (12%)</li>

</ul>

–

Section 5. Supplementary

<ul>

 <li>Homework introduction video</li>

</ul>

–

Section 6. Practice

<ul>

 <li>There are some handwritten questions for everyone to practice on chp4 about midterm exams.</li>

</ul>

<h1>1          Handwritten</h1>

Each 2%.

<h1>2          Programming (86%, including: Pipelined CPU and report)</h1>

<h2>Pipelined CPU</h2>

In this section, we are going to implement a pipeline cpu.

The provided instruction memory is as follows:

<table width="443">

 <tbody>

  <tr>

   <td width="57">Signal</td>

   <td width="57">I/O</td>

   <td width="52">Width</td>

   <td width="277">Functionality</td>

  </tr>

  <tr>

   <td width="57">i clk</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="277">Clock signal</td>

  </tr>

  <tr>

   <td width="57">i rst n</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="277">Active low asynchronous reset</td>

  </tr>

  <tr>

   <td width="57">i valid</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="277">Signal that tells pc-address from cpu is ready</td>

  </tr>

  <tr>

   <td width="57">i addr</td>

   <td width="57">Input</td>

   <td width="52">64</td>

   <td width="277">64-bits address from cpu</td>

  </tr>

  <tr>

   <td width="57">o valid</td>

   <td width="57">Output</td>

   <td width="52">1</td>

   <td width="277">Valid when instruction is ready</td>

  </tr>

  <tr>

   <td width="57">o inst</td>

   <td width="57">Output</td>

   <td width="52">32</td>

   <td width="277">32-bits instruction to cpu</td>

  </tr>

 </tbody>

</table>

And the provided data memory is as follows:

<table width="570">

 <tbody>

  <tr>

   <td width="86">Signal</td>

   <td width="57">I/O</td>

   <td width="52">Width</td>

   <td width="375">Functionality</td>

  </tr>

  <tr>

   <td width="86">i clk</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="375">Clock signal</td>

  </tr>

  <tr>

   <td width="86">i rst n</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="375">Active low asynchronous reset</td>

  </tr>

  <tr>

   <td width="86">i data</td>

   <td width="57">Input</td>

   <td width="52">64</td>

   <td width="375">64-bits data that will be stored</td>

  </tr>

  <tr>

   <td width="86">i w addr</td>

   <td width="57">Input</td>

   <td width="52">64</td>

   <td width="375">Write to target 64-bits address</td>

  </tr>

  <tr>

   <td width="86">i <u>r </u>addr</td>

   <td width="57">Input</td>

   <td width="52">64</td>

   <td width="375">Read from target 64-bits address</td>

  </tr>

  <tr>

   <td width="86">i MemRead</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="375">One cycle signal and set current mode to reading</td>

  </tr>

  <tr>

   <td width="86">i MemWrite</td>

   <td width="57">Input</td>

   <td width="52">1</td>

   <td width="375">One cycle signal and set current mode to writing</td>

  </tr>

  <tr>

   <td width="86">o valid</td>

   <td width="57">Output</td>

   <td width="52">1</td>

   <td width="375">One cycle signal telling data is ready (used when ld happens)</td>

  </tr>

  <tr>

   <td width="86">o data</td>

   <td width="57">Output</td>

   <td width="52">64</td>

   <td width="375">64-bits data from data memory (used when ld happens)</td>

  </tr>

 </tbody>

</table>

The test environment is as follows:

We will only test the instructions highlighted in the red box, as the figures below

And one more instruction to be implemented is

<table width="507">

 <tbody>

  <tr>

   <td width="277">i inst</td>

   <td width="67">Function</td>

   <td width="162">Description</td>

  </tr>

  <tr>

   <td width="277">32’b11111111111111111111111111111111</td>

   <td width="67">Stop</td>

   <td width="162">Stop and set o finish to 1</td>

  </tr>

 </tbody>

</table>

All the environment settings are the same as HW3 except the rule of accessing data_memory.v and instruction_memory.v, and the interface of modules are changed this time. See the supplementary.pdf for more information.

You may want to reference the diagram of pipelined cpu from textbook.

To make sure that pipeline is actually implemented in your design, we are going to use an open source synthesis tool <a href="https://github.com/YosysHQ/yosys">Yosys</a> to check the timing of the critical path in your design. We’ll also use the FreePDK 45 nm process standard cell library provided <a href="https://github.com/cornell-brg/freepdk-45nm">here</a><a href="https://github.com/cornell-brg/freepdk-45nm">.</a>

You can either build Yosys yourself or use the image provided

docker pull ntuca2020/hw4 # size ~ 1.28G docker run –name=test -it ntuca2020/hw4 cd /root ls

Folder structure for this homework:

HW4/

|– testcases/

|             |– generate.s

|              ‘– generate.cpp

|– codes/

|            |– cpu.v

|            |– data_memory.v                           // provided data memory

|                       ‘– instruction_memory.v // provided instruction memory

|– testbench.v

|– Makefile

|– cpu.ys                            // synthesis command

‘– stdcells.lib                        // FreePDK 45 nm standard cell library

Specify all the used modules in the <strong>cpu.ys </strong>file, then run

make                  // Compile

make test // Test all test cases

make time // Show the timing and area used in your design

Information about your design is shown when running make time:

ABC: WireLoad = “none” Gates = 13123 ( 14.8 %)                            Cap = 3.2 ff ( 1.9 %)

Area =              17519.56 ( 87.9 %)                      Delay = 1091.13 ps ( 5.1 %)

You can optimize the cpu for the 3 workloads (code address range, data address range, etc), but it should not affect other test cases.

Grading:

<ul>

 <li>Correctness check (10%)

  <ul>

   <li>10 testcases, each <strong>2% </strong>for correctness check</li>

  </ul></li>

 <li>Required area and frequency (inverse of delay) (32%)

  <ul>

   <li>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>10MHz (5%)</li>

   <li>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>100MHz (5%)</li>

   <li>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>200MHz (5%)</li>

   <li>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>500MHz (5%) <strong>– </strong>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>800MHz (4%)</li>

   <li>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>1000MHz (3%) <strong>– </strong>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>1200MHz (3%)</li>

   <li>Area <em>&lt; </em>25,000 <em>µm</em><sup>2</sup>, <strong>and </strong>frequency <em>&gt; </em>1500MHz (2%)</li>

  </ul></li>

 <li>Required time (clock cycle * operating frequency) to finish workloads from last 3 testcases. (32%)

  <ul>

   <li>Workload1 <em>&lt; </em>100,000 ns (5%)</li>

   <li>Workload2 <em>&lt; </em>150,000 ns (5%)</li>

   <li>Workload3 <em>&lt; </em>200,000 ns (5%)</li>

   <li>Workload1 <em>&lt; </em>10,000 ns (5%)</li>

   <li>Workload2 <em>&lt; </em>15,000 ns (4%)</li>

   <li>Workload3 <em>&lt; </em>20,000 ns (3%)</li>

   <li>Workload1 <em>&lt; </em>5,000 ns, <strong>and </strong>Workload2 <em>&lt; </em>20,000 ns, <strong>and </strong>Workload3 <em>&lt; </em>15,000 ns (3%)</li>

   <li>Workload1 <em>&lt; </em>3,500 ns, <strong>and </strong>Workload2 <em>&lt; </em>9,000 ns, <strong>and </strong>Workload3 <em>&lt; </em>10,000 ns (2%)</li>

  </ul></li>

</ul>