# EXP-3-Adder-sub
## Design and Verification of Half Adder and Half Subtractor
### Aim
To design and verify basic one-bit arithmetic circuits using Verilog HDL.
### Theory / Synopsis
A Half Adder adds two single-bit binary numbers, A and B, and produces a Sum and a Carry output. A Half Subtractor subtracts one single-bit binary number B from another A, producing a Difference and a Borrow output. Both circuits are purely combinational and form the basic building blocks of larger arithmetic units such as Full Adders and Full Subtractors.
Boolean Functions
Half Adder:
•	Sum = A ⊕ B
•	Carry = A . B
Half Subtractor:
•	Difference = A ⊕ B
•	Borrow = A' . B

### Files to be Created
File name	Purpose
half_add_sub.v	Arithmetic-circuit RTL
half_add_sub_tb.v	Stimulus and VCD generation

### Verilog Program
// gedit shift_reg.v
 
module shift_reg (
    input  wire       clk,   // clock
    input  wire       rst,   // asynchronous active-high reset
    input  wire       sin,   // serial input
    output wire       sout,  // serial output (MSB, about to shift out)
    output reg  [3:0] Q      // parallel output
);
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 4'b0000;
        else
            Q <= {Q[2:0], sin};   // shift left, sin enters at LSB
    end
 
    assign sout = Q[3];
endmodule

### Testbench Program

// gedit tb6.v
 
module tb6;
    reg  clk, rst, sin;
    wire sout;
    wire [3:0] Q;
 
    // Instantiate the shift register
    shift_reg uut (
        .clk(clk),
        .rst(rst),
        .sin(sin),
        .sout(sout),
        .Q(Q)
    );
 
    // ---- Clock generation: 10 time-unit period ----
    initial clk = 0;
    always #5 clk = ~clk;
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("shift_reg.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb6);              // dump all signals in this testbench hierarchy
 
        $monitor("Time=%0t rst=%b clk=%b sin=%b | Q=%b sout=%b",
                  $time, rst, clk, sin, Q, sout);
 
        // ---- Apply asynchronous reset ----
        rst = 1; sin = 0;
        #7  rst = 0;
 
        // ---- Serially shift in the bit pattern 1 0 1 1 ----
        @(negedge clk); sin = 1;
        @(negedge clk); sin = 0;
        @(negedge clk); sin = 1;
        @(negedge clk); sin = 1;
 
        // ---- Hold sin = 0 and observe the pattern shift out ----
        @(negedge clk); sin = 0;
        @(negedge clk);
        @(negedge clk);
 
        // ---- Apply reset again mid-simulation ----
        @(negedge clk); rst = 1;
        @(negedge clk); rst = 0; sin = 1;
 
        #20 $finish;
    end
end module
### Truth Table


<img width="645" height="87" alt="TT3" src="https://github.com/user-attachments/assets/37e33bb3-d647-45d5-b610-c255d7e41b8b" />

### Simulation Procedure

STEP 1 – Open Terminal
Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs half_add_sub.v half_add_sub_tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the input combinations and corresponding outputs Sum, Carry, Diff, and Borrow.
A VCD waveform file is also generated:
half_add_sub.vcd

STEP 5 – Open DVE
dve -full64
Other option
dve -full64 &
A DVE environment will open.

DVE Waveform Verification
In DVE:
•	Open the testbench hierarchy.
•	Locate the signals:
o	A
o	B
o	Sum
o	Carry
o	Diff
o	Borrow
•	Add the signals to the waveform window.
•	Run/inspect the waveform.

•	Verify that Sum = 1 whenever A and B are different (A⊕B), and Carry = 1 only when A=1 AND B=1.

•	Verify that Diff = 1 whenever A and B are different (same XOR pattern as Sum), and Borrow = 1 only when A=0 AND B=1.

•	Confirm that Carry and Borrow never both go high for the same input pair, since they represent different (AND vs A'B) conditions.

The waveform should agree with the truth table.

### Expected Result

The Half Adder and Half Subtractor were realized using Verilog HDL:

•	Sum = A⊕B, Carry = A.B
•	Diff = A⊕B, Borrow = A'.B

The design was compiled and simulated using Synopsys VCS, and the functionality was verified using DVE waveform analysis, matching the expected truth table.

### Output
<img width="1895" height="936" alt="Screenshot 2026-11-18 104056" src="https://github.com/user-attachments/assets/7e8139b5-4a89-40ed-a5aa-6b6c281e0666" />

### Viva-Voce Questions

•	What is a Half Adder, and what are its limitations compared to a Full Adder?

•	What is a Half Subtractor, and what does the Borrow output signify?

•	Why is the Sum/Difference expression identical (A⊕B) for both circuits, while Carry and Borrow differ?

•	What gate realizes the XOR function, and why is it central to both circuits?

•	What is the purpose of a Verilog testbench?

•	Why is a VCD file generated, and what does $dumpvars(0, tb) do?

•	What is the purpose of ./simv?

•	What is the difference between simulation and synthesis?

•	How would you extend this design to a Full Adder/Full Subtractor?

### Result

Thus, the Half Adder and Half Subtractor were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.





