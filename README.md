<html lang="en">
<body>
<h1>samsung-riscv</h1>
<h2>Basic Details</h2>
<b>Name:</b> Akshaykumar Tallur
<br>
<b>College:</b> Dayananda Sagar College Of Engineering
<br>
<b>Email:</b> <a href="mailto:tallurakshaykumar@gmail.com">tallurakshaykumar@gmail.com</a>
<br>
<b>GitHub Profile:</b> <a href="https://github.com/akshaykumartallur">akshaykumartallur</a>
<hr>
                                                  <!-- Task 1 -->				  
<details>
<p><summary>
<b>Task 1:</b> Task is to install RISC-V toolchain using VDI link provided,Compiling the C code and Using RISV to get the         corresponding assembly instructions for O1 and Ofast.
</summary></p>
<b>1. Install Ubuntu 18.04 LTS on Oracle Virtual Machine Box and open VDI file provided</b>
<br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/VM_box.png"  alt=Virtual     Machine>
<br><br>
<b>2. Compiling C code</b>
<br><br>
<pre>
cd
gedit sum1ton.c
gcc sum1ton.c
./a.out</pre>
<pre>#include&ltstdio.h&gt
int main(){
		int i, sum=0, n=1000;
			for (i=1;i<=n;++i){
				sum+=i;	}
		printf("Sum of Numbers from 1 to %d is %d\n",n,sum);
return 0;
	}</pre>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/C_code.png"  alt=C code>
<br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/output_of_c_code.png"      alt=commands for c compilation>
<br><br>
<b>3. Object Dump and O1, Ofast Output</b>
<br><br>
<pre><code>
    cat sum1ton.c
    riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
    ls -ltr sum1ton.o
</code></pre>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/assembly_commands.png"    alt=Commands >
<br><br>
<pre><code>riscv64-unknown-elf-objdump -d sum1ton.o |less</code></pre>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/objdump.png" alt=Object dump>
<br><br>
<b> For O1: The number of instructions were 15</b><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/O1_output.png" alt=O1 output>
<br><br>
<b>For Ofast: the number of instructions were 12</b><br><br>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c</code></pre>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/Ofast_output.png"  alt=Ofast output>
<br><br>
</details>
<hr>  
				           <!--End of Task 1-->
                                              <!-- Task 2 -->
                                         <!-- Spike for Sum1ton -->				
<details>
<p><summary>
<b>Task 2:</b> Running the SPIKE Simulation and observe the performance under the -O1 and -Ofast Compiler optimization flags.
</summary></p>
<details>
<p><summary>1. Sum of Integers from 1 to n</summary></p>
<b>Debugging sum1ton.o for O1</b>
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
   101bc:       00008067                ret
</pre>
<p>15 instructions for O1</p>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_O1_sum1ton.png" alt=debugging O1>
<br><br>
<b>Debugging sum1ton.o for Ofast</b>
<pre><p><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
spike -d pk sum1ton.o</code></p></pre>
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
   100dc:       00008067                ret
</pre>
<p>12 instructions for Ofast</p>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_Ofast_sum1ton.png" alt=debugging Ofast>
</details>	   
                                             <!-- Spike for fact -->	   
<details>
<p><summary>2. Factorial of a Number</summary></p>
<b>Compiling Factorial C program</b>
<pre><code>gedit fact.c
gcc fact.c
./a.out</code></pre>
<pre>#inlcude&ltstdio.h&gt
int main(){
               int fact = 1;
               int i = 1;
               int n = 10;
                   while(i<=n){
                       fact*=i;
                       ++i;
                       }
                printf("Factorial of %d is %d\n",n,fact);
        return 0;
                       }
</pre>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Factorial%20Compilation.png", alt=Factorial Compilation>
<br><br>
<b>Debugging fact.o for O1</b>
<pre><p><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o fact.o fact.c
spike pk fact.o
spike -d pk fact.o</code></p></pre>
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
   101dc:       00008067                ret
</pre>
<p>23 instructions for O1</p>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_O1_factorial.png",alt=Debug O1>
<br><br>
<b>Debugging fact.o for Ofast</b>
<pre><p><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o fact.o fact.c
spike pk fact.o
spike -d pk fact.o</code></p></pre>
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
   100dc:       00008067                ret
</pre>
<p>12 instructions for Ofast</p>
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_Ofast_factorial.png",alt=Ofast debug>
<br><br>
</details>
</details>
<hr>   
                                              <!--End of Task 2-->
   </body>
</html>
