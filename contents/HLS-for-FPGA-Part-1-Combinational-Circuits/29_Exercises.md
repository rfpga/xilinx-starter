# 29. Exercises

1- A simple hardware circuit uses the “00001111000011110000” pattern to turn on the 16 LEDs on the Basys3 board.

          a- Describe the hardware in Vivado-HLS and generate the RTL-IP.

          b- Import the design into the Xilinx Vivado suite and generate the FPAG bitstream.

          c- Program the Basys3 board and check the output.


2- A simple hardware circuit drives five lower LEDs (i.e., LD0 to LD4) on the Basys3 board and turns on/off these LEDs using the “01011” pattern.

          a- Describe the hardware in Vivado-HLS and generate the RTL-IP.

          b- Import the design into the Xilinx Vivado suite and generate the FPAG bitstream.

          c- Program the Basys3 board and check the output.


3- The prototype of a C/C++ function is as follows. It is used to drive 16 LEDs on the Basys3 board to show the “0110110110110110” pattern on them.

          a. Complete the HLS description.

          b. Use the Vivado-HLS toolset to generate the corresponding RTL-IP.

          c. Import the design into the Xilinx Vivado suite and generate the FPAG bitstream.

          d. Program the Basys3 board and check the output.



Function prototype:


```
char led_controller(char &lower_leds);
```


---

## Resource

-   [basic_output_exercise_01-vhls.zip](https://rfpga.s3.us-west-1.amazonaws.com/HLS-for-FPGA-Part-1-Combinational-Circuits/resources/basic_output_exercise_01-vhls.zip)

-   [basic_output_exercise_02-vhls.zip](https://rfpga.s3.us-west-1.amazonaws.com/HLS-for-FPGA-Part-1-Combinational-Circuits/resources/basic_output_exercise_02-vhls.zip)

-   [basic_output_exercise_03-vhls.zip](https://rfpga.s3.us-west-1.amazonaws.com/HLS-for-FPGA-Part-1-Combinational-Circuits/resources/basic_output_exercise_03-vhls.zip)

-   [Basic-Output-Exercises-Solution.pdf](https://rfpga.s3.us-west-1.amazonaws.com/HLS-for-FPGA-Part-1-Combinational-Circuits/resources/Basic-Output-Exercises-Solution.pdf)




[Previous](./28_Basis3-Board.md) | [Next]()