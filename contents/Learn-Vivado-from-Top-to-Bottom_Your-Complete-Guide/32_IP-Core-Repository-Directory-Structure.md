# 32. IP Core Repository Directory Structure

The structure of an IP repository is that all of the IP repositories are saved one level under the top level folder, where the repository is found. Take a look at the following location on your PC to see how Xilinx organized their IP cores:

`C:\Xilinx\Vivado\2015.4\data\ip\xilinx` 

Inside their repository it can be seen that every IP core is stored in a separate folder.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/vivado_repo_structure.png" width="60%" > 
    <p align="center">vivado_repo_structure.png</p>
</p>

With this type of setup, it is easy to copy or move your IP repository. Also it is much easier to manage this type of setup as well. You can then create a manage IP project where you would add your custom IP core repository location and then you can set all the details, such as the category for your IP core, name, so and so forth.

It is not required that you have an IP repository, however if you are creating lots of designs or plan on creating more digital design then an IP repository becomes extremely useful.



---

[Previous](./31_Creating-a-Custom-IP-Core-Repository.md) | [Next](./33_Adding-IP-Cores-to-Your-Repository.md)