# 26. AXI Interface Explained

**Introduction to the AXI Interface**

AXI is part of ARM AMBA, a family of micro controller buses first introduced in 1996 [1]. AMBA stands for Advanced Microcontroller Bus Architecture [2].

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/ARM_logo.png" width="90%" > 
</p> 

AXI is an interface that we can implement within our IP cores so that we can communicate with the internal softcore processor or the on chip hardcore processor. Xilinx’s Zynq chip has on board ARM processors that has an AXI interface to communicate with our IP cores. Otherwise if your implementing a soft core processor such as the micro blaze CPU then you will most likely use the AXI interface on the micro blaze softcore processor to control your custom IP cores.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/microblaze_img.png" width="90%" > 
</p> 

AXI4, which is the interface we are using on our IP cores, contains 3 variations which include:

-   AXI4 – used for high performance memory-mapped applications.
-   AXI4 - Lite – used for simple, low throughput memory-mapped communication.
-   AXI4 - Stream – used for high speed streaming data.

**AXI Interface Signals**

Each of the different AXI interfaces utilizes different signals to implement the respective interface. All of the interface signals can be found below [3].

| Signal   | Source Description | Global Signals                   |
| :------- | :----------------- | :------------------------------- | 
| ACLK     | Clock source       | Global clock signal              |
| ARESETn  | Reset source       | Global reset signal, active LOW  |

| Write Address | Channel            | Signals                                                                                                                          |
| :------------ | :----------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| AWID          | Master             | Write address ID. This signal is the identification tag for the write address group of signals.                                  |
| AWADDR        | Master             | Write address. The write address gives the address of the first transfer in a write burst transaction.                           |
| AWLEN         | Master             | Burst length. The burst length gives the exact number of transfers in a burst. This information determines the number of data transfers associated with the address. This changes between AXI3 and AXI4. |
| AWSIZE        | Master             | Burst size. This signal indicates the size of each transfer in the burst.                                                        |
| AWBURST       | Master             | Burst type. The burst type and the size information, determine how the address for each transfer within the burst is calculated. |
| AWLOCK        | Master             | Lock type. Provides additional information about the atomic characteristics of the transfer. This changes between AXI3 and AXI4. |
| AWCACHE       | Master             | Memory type. This signal indicates how transactions are required to progress through a system.                                   |
| AWPROT        | Master             | Protection type. This signal indicates the privilege and security level of the transaction, and whether the transaction is a data access or an instruction access. |
| AWQOS         | Master             | Quality of Service, QoS. The QoS identifier sent for each write transaction. Implemented only in AXI4.                           |
| AWREGION      | Master             | Region identifier. Permits a single physical interface on a slave to be used for multiple logical interfaces.                    |
| AWUSER        | Master             | User signal. Optional User-defined signal in the write address channel. Supported only in AXI4.                                  |
| AWVALID       | Master             | Write address valid. This signal indicates that the channel is signaling valid write address and control information.            |
| AWREADY       | Slave              | Write address ready. This signal indicates that the slave is ready to accept an address and associated control signals.          |

| Write Data    | Channel            | Signals                                                                                                                          |
| :------------ | :----------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| WID           | Master             | Write ID tag. This signal is the ID tag of the write data transfer. Supported only in AXI3.                                      |
| WDATA         | Master             | Write data.                                                                                                                      |
| WSTRB         | Master             | Write strobes. This signal indicates which byte lanes hold valid data. There is one write strobe bit for each eight bits of the write data bus. |
| WLAST         | Master             | Write last. This signal indicates the last transfer in a write burst.                                                            |
| WUSER         | Master             | User signal. Optional User-defined signal in the write data channel. Supported only in AXI4.                                     |
| WVALID        | Master             | Write valid. This signal indicates that valid write data and strobes are available.                                              |
| WREADY        | Slave              | Write ready. This signal indicates that the slave can accept the write data.                                                     |

| Write Response| Channel            | Signals                                                                     |
| :------------ | :----------------- | :-------------------------------------------------------------------------- |
| BID           | Slave              | Response ID tag. This signal is the ID tag of the write response.           |
| BRESP         | Slave              | Write response. This signal indicates the status of the write transaction.  |
| BUSER         | Slave              | User signal. Optional User-defined signal in the write response channel. Supported only in AXI4.  |
| BVALID        | Slave              | Write response valid. This signal indicates that the channel is signaling a valid write response. |
| BREADY        | Master             | Response ready. This signal indicates that the master can accept a write response.                |

| Read Address  | Channel            | Signals                                                                                               |
| :------------ | :----------------- | :---------------------------------------------------------------------------------------------------- |
| ARID          | Master             | Read address ID. This signal is the identification tag for the read address group of signals.         |
| ARADDR        | Master             | Read address. The read address gives the address of the first transfer in a read burst transaction.   |
| ARLEN         | Master             | Burst length. This signal indicates the exact number of transfers in a burst. This changes between AXI3 and AXI4. 
| ARSIZE        | Master             | Burst size. This signal indicates the size of each transfer in the burst.                             |
| ARBURST       | Master             | Burst type. The burst type and the size information determine how the address for each transfer within the burst is calculated. |
| ARLOCK        | Master             | Lock type. This signal provides additional information about the atomic characteristics of the transfer. This changes between AXI3 and AXI4  |
| ARCACHE       | Master             | Memory type. This signal indicates how transactions are required to progress through a system.        |
| ARPROT        | Master             | Protection type. This signal indicates the privilege and security level of the transaction, and whether the transaction is a data access or an instruction access. 
| ARQOS         | Master             | Quality of Service, QoS. QoS identifier sent for each read transaction. Implemented only in AXI4.     |
| ARREGION      | Master             | Region identifier. Permits a single physical interface on a slave to be used for multiple logical interfaces. Implemented only in AXI4.      |
| ARUSER        | Master             | User signal. Optional User-defined signal in the read address channel. Supported only in AXI4.        |
| ARVALID       | Master             | Read address valid. This signal indicates that the channel is signaling valid read address and control information.     |
| ARREADY       | Slave              | Read address ready. This signal indicates that the slave is ready to accept an address and associated control signals.  |

| Read Data     | Channel            | Signals                                                                                                        |
| :------------ | :----------------- | :------------------------------------------------------------------------------------------------------------- |
| RID           | Slave              | Read ID tag. This signal is the identification tag for the read data group of signals generated by the slave.  |
| RDATA         | Slave              | Read data.                                                                                                     |
| RRESP         | Slave              | Read response. This signal indicates the status of the read transfer.                                          |
| RLAST         | Slave              | Read last. This signal indicates the last transfer in a read burst.                                            |
| RUSER         | Slave              | User signal. Optional User-defined signal in the read data channel. Supported only in AXI4.                    |
| RVALID        | Slave              | Read valid. This signal indicates that the channel is signaling the required read data.                        |
| RREADY        | Master             | Read ready. This signal indicates that the master can accept the read data and response information.           |

| Low-power     | Interface          | Signals                                                                                                                                            |
| :------------ | :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| CSYSREQ       | Clock controller   | System exit low-power state request. This signal is a request from the system clock controller for the peripheral to exit from a low-power state.  |
| CSYSACK       | Peripheral device  | Exit low-power state acknowledgement. This signal is the acknowledgement from a peripheral to a system exit low-power state request.               |
| CACTIVE       | Peripheral device  | Clock active. This signal indicates that the peripheral requires its clock signal.                                                                 |

The AXI4 specification includes much more detailed information regarding all the signals used to implement the AXI4 interface. Vivado manages the AXI interface for us so we don’t need to worry about every detail, however getting a background on the topic is very helpful in understand what is actually going on.

**Uses for the AXI Interface?**

We use the AXI interface so that we have a standard way of allowing our custom IP cores to interface with the various processors or other IP cores that we may connect to it. Once an interface has been decided and you have your own custom IP repository, you can easily and quickly create a design using Xilinx’s block designs and dropping in IP cores. The figure below shows an example of a design using the AXI interface.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/AXI_Block_Design.png" width="90%" > 
</p> 


## References

[1] Xilinx UG 761: https://www.xilinx.com/support/documentation/ip_documentation/axi_ref_guide/v13_4/ug761_axi_reference_guide.pdf 

[2] ARM Specifications: http://www.arm.com/products/system-ip/amba-specifications 

[3] AXI4 Specifications:  http://www.gstitt.ece.ufl.edu/courses/fall15/eel4720_5721/labs/refs/AXI4_specification.pdf

---

[Previous](./25_Create-IP-Cores-from-a-Block-Design.md) | [Next](./27_Create-an-AXI-IP-Core-Peripheral-Step-1.md)