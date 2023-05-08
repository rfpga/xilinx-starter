# 40. What are Design Constraints

In this lecture we will be discussing what design constraints are as well as what they are used for. All designs you create that are meant to be implemented onto an FPGA will have some sort of design constraints. The most common form of design constraint is to have the I/O pins mapped from the entity of the HDL design to the actual physical pin locations on the FPGA.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/xdc_file.png" width="90%" > 
</p> 

Design constraints are just as they sound, constraints for your design. We most commonly use them to map the inputs and outputs in the entity of our HDL design to the actual physical pin locations on the FPGA. Aside from mapping physical pins to inputs and outputs on your design, constraints files can also be used for the following:

Specifying the logic level of the input or output signal
Specifying the phase, frequency, and source clock for custom generated clocks
Specify placement constraints for the placement of signals on FPGA I/O pins

Design constraints in Vivado are files that have the extensions .xdc. These are similar to the files in Xilinx’s ISE tool which have the extension .ucf. You can generate constraints files from Vivado’s I/O Pin Planning Tool or you had edit the .xdc file directly. Depending on the development board vendor sometime you can find a master XDC file that has all of the I/O listed out and you just un-comment the peripherals you are using. See the example below:

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/xdc_file_example_master.png" width="90%" > 
</p> 

If you purchase your board from digilent, you can find the master XDC files along with other reference material at digilents reference page:

https://reference.digilentinc.com/reference/programmable-logic/start

## Resource

-   [ug945-vivado-using-constraints-tutorial.pdf](https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/ug945-vivado-using-constraints-tutorial.pdf)

---

[Previous](./39_Using-Vivado's-Connection-Automation-and-Regerating-Block-Design-Layouts.md) | [Next](./41_Applying-IO-Constraints.md)