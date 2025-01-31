<html lang="en"><body>
<h1>samsung-riscv</h1>
<h2>Basic Details</h2>
<b>Name:</b> Akshaykumar Tallur<br>
<b>College:</b> Dayananda Sagar College Of Engineering<br>
<b>Email:</b> <a href="mailto:tallurakshaykumar@gmail.com">tallurakshaykumar@gmail.com</a><br>
<b>GitHub Profile:</b> <a href="https://github.com/akshaykumartallur">akshaykumartallur</a><hr>
<!-- Task 1 -->				  
<details><p><summary><b>Task 1:</b> Task is to install RISC-V toolchain using VDI link provided,Compiling the C code and Using RISV to get the corresponding assembly instructions for O1 and Ofast.</summary></p>
<h3>1. Install Ubuntu 18.04 LTS on Oracle Virtual Machine Box and open VDI file provided</h3><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/VM_box.png"  alt=Virtual     Machine><br><br>
<h3>2. Compiling C code</h3><br><br>
<pre><code>cd
gedit sum1ton.c
gcc sum1ton.c
./a.out</code></pre>
<pre>#include&ltstdio.h&gt
int main(){
		int i, sum=0, n=1000;
			for (i=1;i&lt;=n;++i){
				sum+=i;	}
		printf("Sum of Numbers from 1 to %d is %d\n",n,sum);
return 0;
	}</pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/C_code.png"  alt=C code><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/output_of_c_code.png"      alt=commands for c compilation><br><br>
<h3>3. Object Dump and O1, Ofast Output</h3><br><br>
<pre><code>
    cat sum1ton.c
    riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
    ls -ltr sum1ton.o
</code></pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/assembly_commands.png"    alt=Commands ><br><br>
<pre><code>riscv64-unknown-elf-objdump -d sum1ton.o |less</code></pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/objdump.png" alt=Object dump><br><br>
<b> For O1: The number of instructions were 15</b><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/O1_output.png" alt=O1 output><br><br>
<b>For Ofast: the number of instructions were 12</b><br><br>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c</code></pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/Ofast_output.png"  alt=Ofast output><br><br></details><hr>  
<!--End of Task 1-->
<!-- Task 2 -->
<!-- Spike for Sum1ton -->				
<details><p><summary>
<b>Task 2:</b> Running the SPIKE Simulation and observe the performance under the -O1 and -Ofast Compiler optimization flags.
</summary></p><details>
<p><summary>1. Sum of Integers from 1 to n</summary></p>
<h3>Debugging sum1ton.o for O1</h3>
<pre><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
ls -ltr sum1ton.o
spike pk sum1ton.o
spike -d pk sum1ton.o</code></pre>
<b>O1 assembly output</b>
<pre>0000000000010184 &ltmain&gt:
   10184:       ff010113                addi    sp,sp,-16
   10188:       00113423                sd      ra,8(sp)
   1018c:       3e800793                li      a5,1000
   10190:       fff7879b                addiw   a5,a5,-1
   10194:       fe079ee3                bnez    a5,10190 &ltmain+0xc&gt
   10198:       0007a637                lui     a2,0x7a
   1019c:       31460613                addi    a2,a2,788 # 7a314 &lt;__BSS_END__+0x5710c&gt;
   101a0:       3e800593                li      a1,1000
   101a4:       00021537                lui     a0,0x21
   101a8:       19050513                addi    a0,a0,400 # 21190 &lt;__clzdi2+0x48&gt;
   101ac:       26c000ef                jal     ra,10418 &lt;printf&gt;
   101b0:       00000513                li      a0,0
   101b4:       00813083                ld      ra,8(sp)
   101b8:       01010113                addi    sp,sp,16
   101bc:       00008067                ret</pre>
<p>15 instructions for O1</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_O1_sum1ton.png" alt=debugging O1><br><br>
<h3>Debugging sum1ton.o for Ofast</h3>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
spike -d pk sum1ton.o</code></pre>
<b>Ofast assembly output</b>
<pre>00000000000100b0 &ltmain&gt:
   100b0:       0007a637                lui     a2,0x7a
   100b4:       00021537                lui     a0,0x21
   100b8:       ff010113                addi    sp,sp,-16
   100bc:       31460613                addi    a2,a2,788 # 7a314 &lt;__BSS_END__+0x5710c&gt;
   100c0:       3e800593                li      a1,1000
   100c4:       18050513                addi    a0,a0,384 # 21180 &lt;__clzdi2+0x44&gt;
   100c8:       00113423                sd      ra,8(sp)
   100cc:       340000ef                jal     ra,1040c &lt;printf&gt;
   100d0:       00813083                ld      ra,8(sp)
   100d4:       00000513                li      a0,0
   100d8:       01010113                addi    sp,sp,16
   100dc:       00008067                ret</pre>
<p>12 instructions for Ofast</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_Ofast_sum1ton.png" alt=debugging Ofast>
</details>	   
<!-- Spike for fact -->	   
<details>
<p><summary>2. Factorial of a Number</summary></p>
<h3>Compiling Factorial C program</h3>
<pre><code>gedit fact.c
gcc fact.c
./a.out</code></pre>
<pre>#inlcude&ltstdio.h&gt
int main(){
               int fact = 1;
               int i = 1;
               int n = 10;
                   while(i&lt;=n){
                       fact*=i;
                       ++i;
                       }
                printf("Factorial of %d is %d\n",n,fact);
        return 0;
                       }</pre>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Factorial%20Compilation.png", alt=Factorial Compilation><br><br>
<h3>Debugging fact.o for O1</h3>
<pre><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o fact.o fact.c
spike pk fact.o
spike -d pk fact.o</code></pre>
<b>O1 assembly output</b>
<pre>0000000000010184 &ltmain&gt:
   10184:       fe010113                addi    sp,sp,-32
   10188:       00113c23                sd      ra,24(sp)
   1018c:       00813823                sd      s0,16(sp)
   10190:       00913423                sd      s1,8(sp)
   10194:       00100593                li      a1,1
   10198:       00100413                li      s0,1
   1019c:       00b00493                li      s1,11
   101a0:       00040513                mv      a0,s0
   101a4:       03c000ef                jal     ra,101e0 &lt;__muldi3&gt;
   101a8:       0005059b                sext.w  a1,a0
   101ac:       0014041b                addiw   s0,s0,1
   101b0:       fe9418e3                bne     s0,s1,101a0 &lt;main+0x1c&gt;
   101b4:       00058613                mv      a2,a1
   101b8:       00a00593                li      a1,10
   101bc:       00021537                lui     a0,0x21
   101c0:       1b050513                addi    a0,a0,432 # 211b0 <__clzdi2+0x48>
   101c4:       298000ef                jal     ra,1045c &lt;printf&gt;
   101c8:       00000513                li      a0,0
   101cc:       01813083                ld      ra,24(sp)
   101d0:       01013403                ld      s0,16(sp)
   101d4:       00813483                ld      s1,8(sp)
   101d8:       02010113                addi    sp,sp,32
   101dc:       00008067                ret</pre>
<p>23 instructions for O1</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_O1_factorial.png",alt=Debug O1><br><br>
<h3>Debugging fact.o for Ofast</h3>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o fact.o fact.c
spike pk fact.o
spike -d pk fact.o</code></pre>
<b>Ofast assembly output</b>  
<pre>00000000000100b0 &ltmain&gt:
   100b0:       00376637                lui     a2,0x376
   100b4:       00021537                lui     a0,0x21
   100b8:       ff010113                addi    sp,sp,-16
   100bc:       f0060613                addi    a2,a2,-256 # 375f00 &lt;__BSS_END__+0x352cf8&gt;
   100c0:       00a00593                li      a1,10
   100c4:       18050513                addi    a0,a0,384 # 21180 &lt;__clzdi2+0x44&gt;
   100c8:       00113423                sd      ra,8(sp)
   100cc:       340000ef                jal     ra,1040c &lt;printf&gt;
   100d0:       00813083                ld      ra,8(sp)
   100d4:       00000513                li      a0,0
   100d8:       01010113                addi    sp,sp,16
   100dc:       00008067                ret</pre>
<p>12 instructions for Ofast</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_Ofast_factorial.png",alt=Ofast debug><br><br>
</details></details><hr>   
<!--End of Task 2-->
<!-- Task 3 -->   
<details><summary><b>Task 3:</b> Identify instruction type of all the instructions of the Object dump with its exact 32 bits instruction code in the desired instruction type format.</summary><br>
<details><p><summary>RISC-V Instruction Formats</summary></p>
<!-- Explaination -->	
<h2>Instruction Types and Fields</h2>
<p> The RISC-V instructions are categorized into types based on their filed organization.Each type has specific fields like opcode,funct3,funct4,immediate values and register numbers. The types include:</p>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; R-Type:</b> Register Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; I-Type:</b> Immediate Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; S-Type:</b> Store Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; B-Type:</b> Branch Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; U-Type:</b> Upper Immediate Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; J-Type:</b> Jump Type <br>
<!-- R-Type -->
<h3>RISCV R-Type Instructions</h3>
<p>R-type instructions are used for operations that involve only registers. These instructions typically perform arithmetic, logical, and shift operations.</p>
<b>Format:</b><br>
<pre>
+----------------------------------------------------------------------------------------------------------------------------------+
  funct7[31:25](7-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------------------------------+
</pre>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct7:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2:</b> Second source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>
<!-- I-Type -->
<h3>RISCV I-Type Instructions</h3>
<p>I-Type instructions cover various operations, including immediate arithmetic, load operations, and certain control flow instructions.</p>
<b>Format:</b><br>
<pre>+----------------------------------------------------------------------------------------------------------+
  imm[31:20](12-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------+</pre>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>
<!-- S-Type -->
<h3>RISCV S-Type Instructions</h3>
<p>S-type instructions are essential for accessing and manipulating data in memory.Used to store data from a register to memory.</p>
<b>Format:</b><br>
<pre>+--------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31:25](11:5)(7-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | imm[11:7](4:0)(5-bits) | opcode[6:0](7-bits)
+--------------------------------------------------------------------------------------------------------------------------------------------+</pre>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value( split into imm[11:5] and imm[4:0]).<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2:</b> Second source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>
<!-- B-Type -->   
<h3>RISCV B-Type Instructions</h3>
<p>B-type instructions are crucial for implementing control flow in programs, enabling conditional execution of code blocks.Used for conditional branches, which alter the program flow based on a comparison of register values.</p>
<b>Format:</b><br>
<pre>+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31](12)(1-bit) | imm[30:25](10:5)(6-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | imm[11:8](4:1)(4-bits) | imm[7](11)(1-bit) | opcode[6:0](7-bits)
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+</pre>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value( split into imm[12], imm[10:5], imm[4:1] and imm[11]).<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2:</b> Second source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>
<!-- U-Type -->
<h3>RISCV U-Type Instructions</h3>
<p>U-Type instructions are used for operations like loading upper immediate (LUI) and adding upper immediate to PC (AUIPC).</p>
<b>Format:</b><br>
<pre>+----------------------------------------------------------------------------------------------------------+
                  imm[31:12](20-bits)                |    rd[11:7](5-bits)      |     opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------+</pre>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Upper 20 bits of the immediate value.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>
<!-- J-Type -->    
<h3>RISCV J-Type Instructions</h3>
<p>J-type instructions in RISC-V are primarily used for unconditional jumps to specific target addresses within the program.They play a crucial role in controlling the flow of execution by transferring control to a different part of the code.</p>
<b>Format:</b><br>
<pre>+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31](20)(1-bit) | imm[30:21](10:1)(10-bits) | imm[20](11)(1-bit) | imm[19:12](19:12)(8-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+</pre>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value( split into imm[20], imm[10:1], imm[11] and imm[19:12]).<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br></details>
<!-- Machine Codes -->
<details><p><summary>Machine Codes for Different Instructions</summary></p>
<h2>Machine Codes:</h2>
<pre>0000000000010184 &ltmain&gt:
   10184:       fe010113                addi    sp,sp,-32
   10188:       00113c23                sd      ra,24(sp)
   1018c:       00813823                sd      s0,16(sp)
   10190:       00913423                sd      s1,8(sp)
   10194:       00100593                li      a1,1
   10198:       00100413                li      s0,1
   1019c:       00b00493                li      s1,11
   101a0:       00040513                mv      a0,s0
   101a4:       03c000ef                jal     ra,101e0 &lt;__muldi3&gt;
   101a8:       0005059b                sext.w  a1,a0
   101ac:       0014041b                addiw   s0,s0,1
   101b0:       fe9418e3                bne     s0,s1,101a0 &lt;main+0x1c&gt;
   101b4:       00058613                mv      a2,a1
   101b8:       00a00593                li      a1,10
   101bc:       00021537                lui     a0,0x21
   101c0:       1b050513                addi    a0,a0,432 # 211b0 <__clzdi2+0x48>
   101c4:       298000ef                jal     ra,1045c &lt;printf&gt;
   101c8:       00000513                li      a0,0
   101cc:       01813083                ld      ra,24(sp)
   101d0:       01013403                ld      s0,16(sp)
   101d4:       00813483                ld      s1,8(sp)
   101d8:       02010113                addi    sp,sp,32
   101dc:       00008067                ret</pre>
<!-- 1 -->
<h3>1. Machine code for <code>addi sp, sp, -32</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>addi sp, sp, -32</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>-32 (12 bits,two's complement) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(-32): </b><code>111111100000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>10184:       fe010113          addi  sp, sp, -32</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>111111100000</td>
		<td>00010</td>
		<td>000</td>
		<td>00010</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 2 -->
<h3>2. Machine code for <code>sd ra, 24(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd ra, 24(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0100011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>24 (12 bits split into imm[11:5] and imm[4:0]) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Base Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rd): </b>ra (x1,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(24): </b><code>000000011000 </code>(Split into imm[11:5]=<code>0000000</code> and 		imm[4:0]=<code>11000</code>)<br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2(ra=x1): </b><code>00001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0100011</code> <br><br>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
 	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[11:5] (7 bits): </b><code>0000000</code><br>
  	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2 (5 bits): </b><code>00001</code><br>
   	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1 (5 bits): </b><code>00010</code><br>
    	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3 (3 bits): </b><code>011</code><br>
     	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[4:0] (5 bits): </b><code>11000</code><br>
      	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode (7 bits): </b><code>0100011</code><br><br>
<pre><code>10188:       00113c23       sd   ra, 24(sp)</code></pre>	   
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>00001</td>
		<td>00010</td>
		<td>011</td>
		<td>11000</td>
		<td>0100011</td>
	</tr>
</table>
<!-- 3 -->
<h3>3. Machine code for <code>sd s0, 16(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd s0, 16(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0100011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>16 (12 bits split into imm[11:5] and imm[4:0]) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Base Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(16): </b><code>000000010000 </code>(Split into imm[11:5]=<code>0000000</code> and 		imm[4:0]=<code>10000</code>)<br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0100011</code> <br><br>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
 	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[11:5] (7 bits): </b><code>0000000</code><br>
  	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2 (5 bits): </b><code>01000</code><br>
   	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1 (5 bits): </b><code>00010</code><br>
    	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3 (3 bits): </b><code>011</code><br>
     	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[4:0] (5 bits): </b><code>10000</code><br>
      	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode (7 bits): </b><code>0100011</code><br><br>
<pre><code>1018c:       00813823           sd     s0, 16(sp)</code></pre>	   
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>01000</td>
		<td>00010</td>
		<td>011</td>
		<td>10000</td>
		<td>0100011</td>
	</tr>
</table>
<!-- 4 -->
<h3>4. Machine code for <code>sd s1, 8(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd s1, 8(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0100011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>8 (12 bits split into imm[11:5] and imm[4:0]) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Base Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rd): </b>s1 (x9,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(8): </b><code>000000001000 </code>(Split into imm[11:5]=<code>0000000</code> and 		imm[4:0]=<code>01000</code>)<br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2(s1=x9): </b><code>01001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0100011</code> <br><br>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
 	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[11:5] (7 bits): </b><code>0000000</code><br>
  	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2 (5 bits): </b><code>01001</code><br>
   	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1 (5 bits): </b><code>00010</code><br>
    	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3 (3 bits): </b><code>011</code><br>
     	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[4:0] (5 bits): </b><code>01000</code><br>
      	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode (7 bits): </b><code>0100011</code><br><br>
<pre><code>10190:       00913423           sd    s1, 8(sp)</code></pre>	   
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>01001</td>
		<td>00010</td>
		<td>011</td>
		<td>01000</td>
		<td>0100011</td>
	</tr>
</table>
<!-- 5 -->
<h3>5. Machine code for <code>li a1, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li a1, 1</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>1 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>a1 (x11,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000000001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(a1=x11): </b><code>01011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>10194:       00100593          li    a1, 1</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>00000</td>
		<td>000</td>
		<td>01011</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 6 -->
<h3>6. Machine code for <code>li s0, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s0, 1</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>1 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000000001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>10198:       00100413            li    s0,1</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>00000</td>
		<td>000</td>
		<td>01000</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 7 -->
<h3>7. Machine code for <code>li s1, 11</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s1, 11</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>11 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s1 (x9,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000001011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s1=x9): </b><code>01001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>1019c:       00b00493            li     s1, 11</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000001011</td>
		<td>00000</td>
		<td>000</td>
		<td>01001</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 8 -->
<h3>8. Machine code for <code>mv a0, s0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>mv a0, s0</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>a0 (x10,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0): </b><code>000000000000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(a0=x10): </b><code>01010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101a0:       00040513            mv    a0, s0</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>01000</td>
		<td>000</td>
		<td>01010</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 9 -->
<h3>9. Machine code for <code>sext.w a1, a0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sext.w a1, a0</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0011011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>a0 (x10,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>a1 (x11,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000000000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(a0=x10): </b><code>01010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(a1=x11): </b><code>01011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0011011</code> <br><br>
<pre><code>101a8:       0005059b          sext.w  a1, a0 </code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>01010</td>
		<td>000</td>
		<td>01011</td>
		<td>0011011</td>
	</tr>
</table>
<!-- 10 -->
<h3>10. Machine code for <code>addiw s0, s0, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>addiw s0, s0, 1</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0011011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>1 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000000001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0011011</code> <br><br>
<pre><code>101ac:       0014041b          addiw   s0, s0, 1</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>01000</td>
		<td>000</td>
		<td>01000</td>
		<td>0011011</td>
	</tr>
</table>
<!-- 11 -->
<h3>11. Machine code for <code>lui a0, 0x21</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>lui a0, 0x21</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0110111 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0x21(33) (20 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>a0 (x10,5 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0x21): </b><code>00000000000000100001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(a0=x10): </b><code>01010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0110111</code> <br><br>
<pre><code>101bc:       00021537          lui  a0, 0x21</code></pre>	   
<table>
	<tr>
		<th>Immediate (20 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>00000000000000100001</td>
		<td>01010</td>
		<td>0110111</td>
	</tr>
</table>
<!-- 12 -->
<h3>12. Machine code for <code>ld ra, 24(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld ra, 24(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0000011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>24 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>ra (x1,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(24): </b><code>000000011000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(ra=x1): </b><code>00001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0000011</code> <br><br>
<pre><code>101cc:       01813083          ld   ra, 24(sp)</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000011000</td>
		<td>00010</td>
		<td>011</td>
		<td>00001</td>
		<td>0000011</td>
	</tr>
</table>
<!-- 13 -->
<h3>13. Machine code for <code>ld s0, 16(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld s0, 16(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0000011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>16 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(16): </b><code>000000010000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0000011</code> <br><br>
<pre><code>101d0:       01013403          ld   s0, 16(sp)</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000010000</td>
		<td>00010</td>
		<td>011</td>
		<td>01000</td>
		<td>0000011</td>
	</tr>
</table>
<!-- 14 -->
<h3>14. Machine code for <code>ld s1, 8(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld s1, 8(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0000011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>8 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s1 (x9,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(8): </b><code>000000001000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s1=x9): </b><code>01001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0000011</code> <br><br>
<pre><code>101d4:       00813483          ld   s1, 8(sp)</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000001000</td>
		<td>00010</td>
		<td>011</td>
		<td>01001</td>
		<td>0000011</td>
	</tr>
</table>
<!-- 15 -->
<h3>15. Machine code for <code>ret</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ret</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>1100111 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>ra (x1,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000001011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(ra=x1): </b><code>00001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>1100111</code> <br><br>
<pre><code>101dc:       00008067       ret</code></pre>	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>00001</td>
		<td>000</td>
		<td>00000</td>
		<td>1100111</td>
	</tr>
</table>
</details>
</details>
<hr>
<!--End of Task 3-->
<!-- Task 4 -->
<details><summary><b>Task 4: </b>By using RISC-V Core: Verilog netlist and Testbench, perform an experiment of Functional Simulation using GTKWave and Observe the waveforms.</summary>
<h3>Steps:</h3>
1. Using suitable commands install the iverilog and GTKWave in ubuntu<br>
2. Compile the RISC-V Core: Verilog netlist and Testbench<br>
3. Observe the waveform output in GTKWave window<br>
<h4>Installing iverilog and GTKWave in Ubuntu:</h4>
<pre><code>sudo apt install iverilog gtkwave</code></pre>
<h3>Simulate and run the verilog code</h3>
<pre><code>iverilog -o iiitb_rv32i iiitb_rv32i.v iiitb_rv32i_tb.v
./iiitb_rv32i
gtkwave iiitb_rv32i.vcd</code></pre>
<h4>GTKWave Window:</h4><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/GTKWave_Window.png" alt="GTKWave Window">
<br><br>
<h4>Hardcoded Instructions:</h4><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/Instructions.png" alt="Hardcoded ISA">
<br>
<h3>Ouput Waveforms:</h3>
<p>The output waveforms showing the instructions performed in a 5-stage pipelined architecture</p>
<b><i>Instruction 1:</i></b><pre> ADD R6, R2, R1</pre>
	<p>This instruction Adds values of registers R2 and R1 and stores the result in register R6, In this case 1 + 2 = 3.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/01_add_r6_r1_r2.png" alt="ADD R6, R2, R1">
<br><br><b><i>Instruction 2:</i></b><pre> SUB R7, R1, R2</pre>
	<p>This instruction subtracts value of register R2 from R1 and stores the result in register R7, In this case 1 - 2 = -1.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/02_sub_r7_r1_r2.png" alt="SUB R7, R1, R2">
<br><br><b><i>Instruction 3:</i></b><pre> AND R8, R1, R3</pre>
	<p>This instruction executes bitwise "AND" between values of registers R1 and R3 and stores the result in register R8, In this case 01 & 11 = 01(1 in decimal).</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/03_and_r8%2Cr1%2Cr3.png" alt="AND R8, R1, R3">
<br><br><b><i>Instruction 4:</i></b><pre> OR R9, R2, R5</pre>
	<p>This instruction executes bitwise "OR" between values of registers R2 and R5 and stores the result in register R9, In this case 010 | 101 = 111(7 in decimal).</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/04_or_r9_r2_r5.png" alt="OR R9, R2, R5">
<br><br><b><i>Instruction 5:</i></b><pre> XOR R10, R1, R4</pre>
	<p>This instruction executes bitwise XOR between values of registers R1 and R4 and stores the result in register R10, In this case 001 ^ 100 = 101(5 in decimal).</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/05_xor_r10_r1_r4.png" alt="XOR R10, R1, R4">
<br><br><b><i>Instruction 6:</i></b><pre> SLT R11, R2, R4</pre>
	<p>This instruction checks the values of registers R2 and R4 if value of R2 is less than value of R4, then register R11 is set to 1, In this case 2<4 so R11 is set to 1.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/06_slt_r11_r2_r4.png" alt="SLT R11, R2, R4">
<br><br><b><i>Instruction 7:</i></b><pre> ADDI R12, R4, 5</pre>
	<p>This instruction adds the immediate data 5 to the value in register R4 and stores the result in register R12, In this case 4 + 5 = 9.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/07_addi_r12_r4_5.png" alt="ADDI R12, R4, 5">
<br><br><b><i>Instruction 8:</i></b><pre> SW R3, R1, 2</pre>
	<p>This instruction stores the register data @R1+2 into the memory, In this case 1 + 2 = 3.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/08_sw_r3_r1_2.png" alt="SW R3, R1, 2">
<br><br><b><i>Instruction 9:</i></b><pre> LW R13, R1, 2</pre>
	<p>This instruction loads the register data @R1+2 into the register R13, In this case 1 + 2 = 3.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/09_lw_r13_r1_2.png" alt="LW R13, R1, 2">
<br><br><b><i>Instruction 10:</i></b><pre> BEQ R0, R0, 15</pre>
	<p>This instruction Branches to 15 instructions ahead of current instruction if values of registers R0 equals R0, so Program Counter will be incremented by 15, In this case PC is 10 so new PC value will be 10+15=25.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/10_beq_r0_r0_15.png" alt="BEQ R0, R0, 15">
<br><br><b><i>Instruction 11:</i></b><pre> ADD R14, R2 R2</pre>
	<p> This instruction Adds values of registers R2 and R2 and stores the result in register R14, In this case 2 + 2 = 4.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/11_add_r14_r2_r2.png" alt="ADD R14, R2 R2">
<br><br><b><i>Instruction 12:</i></b><pre> BNE R0, R1, 20</pre>
	<p>This instruction Branches to 20 instructions ahead of current instruction if values of registers R0 and R1 don't match , so Program Counter will be incremented by 20, In this case PC is 28 so new PC value will be 28+20=48.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/12_bne_r0_r1_20.png" alt="BNE R0, R1, 20">
<br><br><b><i>Instruction 13:</i></b><pre> ADDI R12, R4, 5</pre>
	<p>This instruction adds the immediate data 5 to the value in register R4 and stores the result in register R12, In this case 4 + 5 = 9.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/13_addi_r12_r4_5.png" alt="ADDI R12, R4, 5">
<br><br><b><i>Instruction 14:</i></b><pre> SLL R15, R1, R2</pre>
	<p>This instruction shifts the value of register R1 to left by 2, (001)&lt;&lt;2=(100)4.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/14_sll_r15_r1_r2.png" alt="SLL R15, R1, R2">
<br><br><b><i>Instruction 15:</i></b><pre> SRL R16, R4, R2</pre>
	<p>This instruction shifts the value of register R1 to right by 2, (100)&gt;&gt;2=(001)1.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/15_srl_r16_r4_r2.png" alt="SRL R16, R4, R2">
<br><br>
</details>
<!--End of Task 4-->
<hr>
</body>
</html>
