<h1>samsung-riscv</h1>
<h2>Basic Details</h2>
<b>Name:</b> Akshaykumar Tallur
<br>
<b>College:</b> Dayananda Sagar College Of Engineering
<br>
<b>Email:</b> <a href="tallurakshaykumar@gmail.com">tallurakshaykumar@gmail.com</a>
<br>
<b>GitHub Profile:</b> <a href="https://github.com/akshaykumartallur">akshaykumartallur</a>
<hr>
<!-- Task 1 -->
    <details>
      <p><summary>
      <b>Task 1:</b> Task is to install RISC-V toolchain using VDI link provided,Compiling the C code and Using RISV to get the         corresponding assembly instructions for O1 and Ofast
    </summary></p>
    <b>1. Install Ubuntu 18.04 LTS on Oracle Virtual Machine Box and open VDI file provided</b>
    <br><br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/virtualmachine.jpg"  alt=Virtual     Machine>
    <br><br>
    <b>2. Compiling C code</b>
    <br><br>
    <pre><code>
    cd
    gedit sum1ton.c
    gcc sum1ton.c
    ./a.out</code></pre>
    <br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/code.jpg"  alt=C code>
    <br><br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/codeandcompilation.jpg"      alt=commands for c compilation>
    <br><br>
    <b>3. Object Dump and O1, Ofast Output</b>
    <br><br>
    <pre><code>
    cat sum1ton.c
    riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
    ls -ltr sum1ton.o
    </code></pre>
    <br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/commands%20for%20assembly%20code.jpg"    alt=Commands >
    <br><br>
    <pre><code>riscv64-unknown-elf-objdump -d sum1ton.o |less</code></pre>
    <br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/object%20dump.jpg" alt=Object dump>
      <br><br>
        <b> For O1: The number of instructions were 15</b><br><br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/assembly%20code%20for%20O1.jpg" alt=O1 output>
    <br><br>
        <b>For Ofast: the number of instructions were 12</b><br><br>
    <pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c</code></pre>
    <br>
    <img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/assembly%20code%20for%20Ofast.jpg"  alt=Ofast output>
    <br><br>
    </details>
<hr>
<!--End of Task 1-->

