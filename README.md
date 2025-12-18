## vivado_factorial
## REG NUM :25014334
## NAME :Santhiya B
## EXPERIMENT:4 FACTORIAL OF N USING FUNCTION (VERILOG)
## TITLE
Design and Implementation of Factorial of N Using Verilog Function on Boolean Board (Spartan-7 FPGA)

## AIM
To design a Verilog program that computes the factorial of a given number using a function, and display the result on the LEDs of the Boolean Board (Spartan-7 FPGA) using Vivado.

## TOOLS REQUIRED
Vivado Design Suite
Boolean Board (Spartan-7 XC7S50)
USB Cable for programming
Personal computer

## THEORY
A factorial function is defined as:
n!=n×(n−1)×(n−2)×...×1
Since the Boolean board has 8 LEDs, we can display results 0–255 (8-bit).
Hence factorial above 5! = 120 is OK, but 6!, 7! overflow 8-bit range.
In Verilog, a function is used to compute factorial inside combinational logic.

## VERILOG PROGRAM (factorial.v)
```
module factorial(
    input  [2:0] n,       // input number 0–7
    output reg [7:0] fact
);

    // factorial function
    function [7:0] factorial_func;
        input [2:0] x;
        integer i;
        begin
            factorial_func = 1;
            for(i = 1; i <= x; i = i + 1)
                factorial_func = factorial_func * i;
        end
    endfunction

    always @(*) begin
        fact = factorial_func(n);
    end
endmodule
```
## TOP MODULE (top.v — used for FPGA hardware)
```
module top(
    input  [2:0] SW,     // Boolean board switches
    output [7:0] LED     // Boolean board LEDs
);

    factorial U1(
        .n(SW),
        .fact(LED)
    );

endmodule
```
## TESTBENCH (tb_factorial.sv)
```
For simulation only (not used in FPGA hardware)
module tb_factorial;

    reg  [2:0] n;
    wire [7:0] fact;

    factorial DUT (
        .n(n),
        .fact(fact)
    );

    initial begin
        $display("N → FACTORIAL");

        n = 0; #10; $display("%d → %d", n, fact);
        n = 1; #10; $display("%d → %d", n, fact);
        n = 2; #10; $display("%d → %d", n, fact);
        n = 3; #10; $display("%d → %d", n, fact);
        n = 4; #10; $display("%d → %d", n, fact);
        n = 5; #10; $display("%d → %d", n, fact);
        n = 6; #10; $display("%d → %d", n, fact);
        n = 7; #10; $display("%d → %d", n, fact);

        $stop;
    end

endmodule
```
## XDC FILE (Boolean Board Pin Mapping)
Use only 3 switches → SW[2:0]
Use 8 LEDs → LED[7:0]
```
# INPUT SWITCHES  (SW[2:0])
set_property -dict { PACKAGE_PIN V2 IOSTANDARD LVCMOS33 } [get_ports {SW[0]}]
set_property -dict { PACKAGE_PIN U2 IOSTANDARD LVCMOS33 } [get_ports {SW[1]}]
set_property -dict { PACKAGE_PIN U1 IOSTANDARD LVCMOS33 } [get_ports {SW[2]}]
# OUTPUT LED[7:0]
set_property -dict { PACKAGE_PIN G1 IOSTANDARD LVCMOS33 } [get_ports {LED[0]}]
set_property -dict { PACKAGE_PIN G2 IOSTANDARD LVCMOS33 } [get_ports {LED[1]}]
set_property -dict { PACKAGE_PIN F1 IOSTANDARD LVCMOS33 } [get_ports {LED[2]}]
set_property -dict { PACKAGE_PIN F2 IOSTANDARD LVCMOS33 } [get_ports {LED[3]}]
set_property -dict { PACKAGE_PIN E1 IOSTANDARD LVCMOS33 } [get_ports {LED[4]}]
set_property -dict { PACKAGE_PIN E2 IOSTANDARD LVCMOS33 } [get_ports {LED[5]}]
set_property -dict { PACKAGE_PIN E3 IOSTANDARD LVCMOS33 } [get_ports {LED[6]}]
set_property -dict { PACKAGE_PIN E5 IOSTANDARD LVCMOS33 } [get_ports {LED[7]}]
```
## PROCEDURE
In Vivado
Create a new project → RTL project
Add Verilog files:
factorial.v
top.v
Add testbench file (tb_factorial.sv) to simulation sources
Add XDC file
Set top.v as Top Module
Run Simulation (for waveforms & results)
Run Synthesis → Implementation → Generate Bitstream
Program the Boolean Board
Toggle switches SW2 SW1 SW0 to give input N

## OUTPUT
<img width="975" height="580" alt="image" src="https://github.com/user-attachments/assets/f951fabc-1200-47f9-bd81-9b6a784a0dee" />

## RESULT
The factorial of the input number provided using the Boolean Board switches is successfully computed using a Verilog function and displayed on the LEDs.
The program is implemented and tested both in simulation and FPGA hardware
