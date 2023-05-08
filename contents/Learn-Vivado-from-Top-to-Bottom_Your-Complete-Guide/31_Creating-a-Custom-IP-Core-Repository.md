# 31. Creating a Custom IP Core Repository

Introduction

An IP core repository is a directory on your computer that contains files that are used to represent your hardware designs. When creating an IP core repository, the location of the repository is the most important decision. However, you can easily move your IP core repositories, the issue is that you will need to update the repository links in all the projects that are referencing that specific IP core repository.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/my_ip_repo.png" width="60%" > 
    <p align="center">my_ip_repo.png</p>
</p>

**File Structure**

The file structure of your IP core repository is such that all your IP cores are located one layer under the repository folder, as seen in the Figure below.

`<repo folder structure>`

Once you have the repository specified you will want to start saving all of your IP cores in this location. Then when you create a IP management project you will be able to edit and manage all of your IP cores.


---

[Previous](./30_Customizing-IP-Cores.md) | [Next](./32_IP-Core-Repository-Directory-Structure.md)