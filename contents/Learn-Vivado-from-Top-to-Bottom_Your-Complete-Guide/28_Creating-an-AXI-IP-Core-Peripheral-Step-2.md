# 28. Creating an AXI IP Core Peripheral - Step 2

In this article I am going to explain how to take your hardware design and integrate it with the AXI IP core files you just created. Vivado creates a shell that incorporates the AXI interface, were the number of registers implemented are based on the parameters you specified in the previous step. These registers are what you want to plug directly into your IP core inputs. In our case we have the inputs seg0, seg1, seg2, seg3, and the anode inputs that all have a separate register associated with them. Figure 1 below shows a block diagram of how the registers are interfaced with the actual design.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/interface_block_diagram.png" width="60%" > 
    <p align="center">Block DiagramFigure 1 - AXI Memory Mapped Interface Diagram</p>
</p> 

In order to integrate your design, first you need to navigate to the folder selected when going through the IP Core wizard in step 1, you should see a directory structure as shown below in Figure 2:

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/directory_structure.png" width="60%" > 
    <p align="center">Directory StructureFigure 2 - Directory Structure</p>
</p> 

Next navigate to the hdlfolder and there you should see two hdl files, in my case vhdl files, as seen below in Figure 3:

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/vhdl_files.png" width="60%" > 
    <p align="center">HDL FilesFigure 3 - HDL File names</p>

The BASYS_7_seg_AXI_v1_0_S00_AXI.vhd is the actual design where the AXI interface is described as well as the location where we are placing our custom design that is incorporating the AXI interface. Open the BASYS_7_seg_AXI_v1_0_S00_AXI.vhd file and add your custom generics and ports required by your design. The BASYS_7_seg_AXI_v1_0_S00_AXI design has comments that explain where to add your customizations. Figure 4 shows the BASYS_7_seg_AXI_v1_0_S00_AXI design and how it shows were to add your custom logic.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/BASYS_7_seg_AXI_v1_0_S00_AXI.png" width="60%" > 
    <p align="center">BASYS_7_seg_AXI_v1_0_S00_AXIFigure 4 </p>
</p> 

The BASYS_7_seg_AXI_v1_0.vhd file contains the component instantiations of both the AXI interface as well as the BASYS_7_seg_AXI_v1_0_S00_AXI component. Inside the BASYS_7_seg_AXI_v1_0 design we want to add any generics and external interfaces that our IP core needs. Figure 5 shows the BASYS_7_seg_AXI_v1_0 design and how it shows were to add your custom logic.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/BASYS_7_seg_AXI_v1_0.png" width="60%" > 
    <p align="center">BASYS_7_seg_AXI_v1_0Figure 5</p>
</p> 

BASYS_7_seg_AXI_v1_0Figure 5 - BASYS_7_seg_AXI_v1_0 Design
The completed BASYS_7_seg_AXI_v1_0 design can be found below:

```
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;
entity BASYS_7_seg_AXI_v1_0 is
 generic (
 -- Users to add parameters here
 INPUT_CLK_FREQ : integer := 100000000;
 REFRESH_RATE : integer := 50000;
 -- User parameters ends
 -- Do not modify the parameters beyond this line
 -- Parameters of Axi Slave Bus Interface S00_AXI
 C_S00_AXI_DATA_WIDTH : integer := 32;
 C_S00_AXI_ADDR_WIDTH : integer := 5
 );
 port (
 -- Users to add ports here
 SEG_OUT : out std_logic_vector(6 downto 0);
 DP_OUT : out std_logic;
 AN_OUT : out std_logic_vector(3 downto 0);
 CLK : in std_logic;
 -- User ports ends
 -- Do not modify the ports beyond this line
 -- Ports of Axi Slave Bus Interface S00_AXI
 s00_axi_aclk : in std_logic;
 s00_axi_aresetn : in std_logic;
 s00_axi_awaddr : in std_logic_vector(C_S00_AXI_ADDR_WIDTH-1 downto 0);
 s00_axi_awprot : in std_logic_vector(2 downto 0);
 s00_axi_awvalid : in std_logic;
 s00_axi_awready : out std_logic;
 s00_axi_wdata : in std_logic_vector(C_S00_AXI_DATA_WIDTH-1 downto 0);
 s00_axi_wstrb : in std_logic_vector((C_S00_AXI_DATA_WIDTH/8)-1 downto 0);
 s00_axi_wvalid : in std_logic;
 s00_axi_wready : out std_logic;
 s00_axi_bresp : out std_logic_vector(1 downto 0);
 s00_axi_bvalid : out std_logic;
 s00_axi_bready : in std_logic;
 s00_axi_araddr : in std_logic_vector(C_S00_AXI_ADDR_WIDTH-1 downto 0);
 s00_axi_arprot : in std_logic_vector(2 downto 0);
 s00_axi_arvalid : in std_logic;
 s00_axi_arready : out std_logic;
 s00_axi_rdata : out std_logic_vector(C_S00_AXI_DATA_WIDTH-1 downto 0);
 s00_axi_rresp : out std_logic_vector(1 downto 0);
 s00_axi_rvalid : out std_logic;
 s00_axi_rready : in std_logic
 );
end BASYS_7_seg_AXI_v1_0;
architecture arch_imp of BASYS_7_seg_AXI_v1_0 is
 -- component declaration
 component BASYS_7_seg_AXI_v1_0_S00_AXI is
 generic (
 INPUT_CLK_FREQ : integer := 100000000;
 REFRESH_RATE : integer := 50000;
 C_S_AXI_DATA_WIDTH : integer := 32;
 C_S_AXI_ADDR_WIDTH : integer := 5
 );
 port (
 SEG_OUT : out std_logic_vector(6 downto 0);
 DP_OUT : out std_logic;
 AN_OUT : out std_logic_vector(3 downto 0);
 CLK : in std_logic;
 S_AXI_ACLK : in std_logic;
 S_AXI_ARESETN : in std_logic;
 S_AXI_AWADDR : in std_logic_vector(C_S_AXI_ADDR_WIDTH-1 downto 0);
 S_AXI_AWPROT : in std_logic_vector(2 downto 0);
 S_AXI_AWVALID : in std_logic;
 S_AXI_AWREADY : out std_logic;
 S_AXI_WDATA : in std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 S_AXI_WSTRB : in std_logic_vector((C_S_AXI_DATA_WIDTH/8)-1 downto 0);
 S_AXI_WVALID : in std_logic;
 S_AXI_WREADY : out std_logic;
 S_AXI_BRESP : out std_logic_vector(1 downto 0);
 S_AXI_BVALID : out std_logic;
 S_AXI_BREADY : in std_logic;
 S_AXI_ARADDR : in std_logic_vector(C_S_AXI_ADDR_WIDTH-1 downto 0);
 S_AXI_ARPROT : in std_logic_vector(2 downto 0);
 S_AXI_ARVALID : in std_logic;
 S_AXI_ARREADY : out std_logic;
 S_AXI_RDATA : out std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 S_AXI_RRESP : out std_logic_vector(1 downto 0);
 S_AXI_RVALID : out std_logic;
 S_AXI_RREADY : in std_logic
 );
 end component BASYS_7_seg_AXI_v1_0_S00_AXI;
begin
-- Instantiation of Axi Bus Interface S00_AXI
BASYS_7_seg_AXI_v1_0_S00_AXI_inst : BASYS_7_seg_AXI_v1_0_S00_AXI
 generic map (
 INPUT_CLK_FREQ,
 REFRESH_RATE,
 C_S_AXI_DATA_WIDTH => C_S00_AXI_DATA_WIDTH,
 C_S_AXI_ADDR_WIDTH => C_S00_AXI_ADDR_WIDTH
 )
 port map (
 SEG_OUT,
 DP_OUT,
 AN_OUT,
 CLK,
 S_AXI_ACLK => s00_axi_aclk,
 S_AXI_ARESETN => s00_axi_aresetn,
 S_AXI_AWADDR => s00_axi_awaddr,
 S_AXI_AWPROT => s00_axi_awprot,
 S_AXI_AWVALID => s00_axi_awvalid,
 S_AXI_AWREADY => s00_axi_awready,
 S_AXI_WDATA => s00_axi_wdata,
 S_AXI_WSTRB => s00_axi_wstrb,
 S_AXI_WVALID => s00_axi_wvalid,
 S_AXI_WREADY => s00_axi_wready,
 S_AXI_BRESP => s00_axi_bresp,
 S_AXI_BVALID => s00_axi_bvalid,
 S_AXI_BREADY => s00_axi_bready,
 S_AXI_ARADDR => s00_axi_araddr,
 S_AXI_ARPROT => s00_axi_arprot,
 S_AXI_ARVALID => s00_axi_arvalid,
 S_AXI_ARREADY => s00_axi_arready,
 S_AXI_RDATA => s00_axi_rdata,
 S_AXI_RRESP => s00_axi_rresp,
 S_AXI_RVALID => s00_axi_rvalid,
 S_AXI_RREADY => s00_axi_rready
 );
 -- Add user logic here
 -- User logic ends
end arch_imp;
```

The completed BASYS_7_seg_AXI_v1_0_S00_AXI design can be found below:

```
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;
entity BASYS_7_seg_AXI_v1_0_S00_AXI is
 generic (
 -- Users to add parameters here
 -- Input Clock Frequency (Hz)
 INPUT_CLK_FREQ : integer := 100000000;
 -- Refresh Rate of display (Hz)
 REFRESH_RATE : integer := 50000;
 -- User parameters ends
 -- Do not modify the parameters beyond this line
 -- Width of S_AXI data bus
 C_S_AXI_DATA_WIDTH : integer := 32;
 -- Width of S_AXI address bus
 C_S_AXI_ADDR_WIDTH : integer := 5
 );
 port (
 -- Users to add ports here
 SEG_OUT : out std_logic_vector(6 downto 0);
 DP_OUT : out std_logic;
 AN_OUT : out std_logic_vector(3 downto 0);
 CLK : in std_logic;
 -- User ports ends
 -- Do not modify the ports beyond this line
 -- Global Clock Signal
 S_AXI_ACLK : in std_logic;
 -- Global Reset Signal. This Signal is Active LOW
 S_AXI_ARESETN : in std_logic;
 -- Write address (issued by master, acceped by Slave)
 S_AXI_AWADDR : in std_logic_vector(C_S_AXI_ADDR_WIDTH-1 downto 0);
 -- Write channel Protection type. This signal indicates the
    -- privilege and security level of the transaction, and whether
    -- the transaction is a data access or an instruction access.
 S_AXI_AWPROT : in std_logic_vector(2 downto 0);
 -- Write address valid. This signal indicates that the master signaling
    -- valid write address and control information.
 S_AXI_AWVALID : in std_logic;
 -- Write address ready. This signal indicates that the slave is ready
    -- to accept an address and associated control signals.
 S_AXI_AWREADY : out std_logic;
 -- Write data (issued by master, acceped by Slave) 
 S_AXI_WDATA : in std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 -- Write strobes. This signal indicates which byte lanes hold
    -- valid data. There is one write strobe bit for each eight
    -- bits of the write data bus.    
 S_AXI_WSTRB : in std_logic_vector((C_S_AXI_DATA_WIDTH/8)-1 downto 0);
 -- Write valid. This signal indicates that valid write
    -- data and strobes are available.
 S_AXI_WVALID : in std_logic;
 -- Write ready. This signal indicates that the slave
    -- can accept the write data.
 S_AXI_WREADY : out std_logic;
 -- Write response. This signal indicates the status
    -- of the write transaction.
 S_AXI_BRESP : out std_logic_vector(1 downto 0);
 -- Write response valid. This signal indicates that the channel
    -- is signaling a valid write response.
 S_AXI_BVALID : out std_logic;
 -- Response ready. This signal indicates that the master
    -- can accept a write response.
 S_AXI_BREADY : in std_logic;
 -- Read address (issued by master, acceped by Slave)
 S_AXI_ARADDR : in std_logic_vector(C_S_AXI_ADDR_WIDTH-1 downto 0);
 -- Protection type. This signal indicates the privilege
    -- and security level of the transaction, and whether the
    -- transaction is a data access or an instruction access.
 S_AXI_ARPROT : in std_logic_vector(2 downto 0);
 -- Read address valid. This signal indicates that the channel
    -- is signaling valid read address and control information.
 S_AXI_ARVALID : in std_logic;
 -- Read address ready. This signal indicates that the slave is
    -- ready to accept an address and associated control signals.
 S_AXI_ARREADY : out std_logic;
 -- Read data (issued by slave)
 S_AXI_RDATA : out std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 -- Read response. This signal indicates the status of the
    -- read transfer.
 S_AXI_RRESP : out std_logic_vector(1 downto 0);
 -- Read valid. This signal indicates that the channel is
    -- signaling the required read data.
 S_AXI_RVALID : out std_logic;
 -- Read ready. This signal indicates that the master can
    -- accept the read data and response information.
 S_AXI_RREADY : in std_logic
 );
end BASYS_7_seg_AXI_v1_0_S00_AXI;
architecture arch_imp of BASYS_7_seg_AXI_v1_0_S00_AXI is
 -- BASYS_7_seg Component
 component BASYS_7_seg is
 generic(
 input_clk_freq : integer := 100000000; -- Input clock rate in Hz
 refresh_rate : integer := 50000);    -- Refresh rate in Hz
 port(
 -- 7 Segment Display Output
 seg : out std_logic_vector(6 downto 0); 
 -- 7 Segment Display Decimal Point
 dp : out std_logic;
 -- Selects Digit 
 an : out std_logic_vector(3 downto 0);
 -- Input segments 0 through 3
 seg0 : in std_logic_vector(6 downto 0);
 seg1 : in std_logic_vector(6 downto 0);
 seg2 : in std_logic_vector(6 downto 0);
 seg3 : in std_logic_vector(6 downto 0);
 -- Input decimal points
 in_dp : in std_logic_vector(3 downto 0);
 -- Input Clock
 clk : in std_logic);
 end component BASYS_7_seg;
 -- AXI4LITE signals
 signal axi_awaddr : std_logic_vector(C_S_AXI_ADDR_WIDTH-1 downto 0);
 signal axi_awready : std_logic;
 signal axi_wready : std_logic;
 signal axi_bresp : std_logic_vector(1 downto 0);
 signal axi_bvalid : std_logic;
 signal axi_araddr : std_logic_vector(C_S_AXI_ADDR_WIDTH-1 downto 0);
 signal axi_arready : std_logic;
 signal axi_rdata : std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal axi_rresp : std_logic_vector(1 downto 0);
 signal axi_rvalid : std_logic;
 -- Example-specific design signals
 -- local parameter for addressing 32 bit / 64 bit C_S_AXI_DATA_WIDTH
 -- ADDR_LSB is used for addressing 32/64 bit registers/memories
 -- ADDR_LSB = 2 for 32 bits (n downto 2)
 -- ADDR_LSB = 3 for 64 bits (n downto 3)
 constant ADDR_LSB  : integer := (C_S_AXI_DATA_WIDTH/32)+ 1;
 constant OPT_MEM_ADDR_BITS : integer := 2;
 ------------------------------------------------
 ---- Signals for user logic register space example
 --------------------------------------------------
 ---- Number of Slave Registers 5
 signal slv_reg0 :std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal slv_reg1 :std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal slv_reg2 :std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal slv_reg3 :std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal slv_reg4 :std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal slv_reg_rden : std_logic;
 signal slv_reg_wren : std_logic;
 signal reg_data_out :std_logic_vector(C_S_AXI_DATA_WIDTH-1 downto 0);
 signal byte_index : integer;
begin
 -- Seg Display Instantiation
 seg_display_inst: BASYS_7_seg
 generic map(INPUT_CLK_FREQ, REFRESH_RATE)
 port map(SEG_OUT, DP_OUT, AN_OUT, slv_reg0(6 downto 0), slv_reg1(6 downto 0),
 slv_reg2(6 downto 0), slv_reg3(6 downto 0), slv_reg4(3 downto 0), CLK);
  
 -- I/O Connections assignments
 S_AXI_AWREADY <= axi_awready;
 S_AXI_WREADY <= axi_wready;
 S_AXI_BRESP <= axi_bresp;
 S_AXI_BVALID <= axi_bvalid;
 S_AXI_ARREADY <= axi_arready;
 S_AXI_RDATA <= axi_rdata;
 S_AXI_RRESP <= axi_rresp;
 S_AXI_RVALID <= axi_rvalid;
 -- Implement axi_awready generation
 -- axi_awready is asserted for one S_AXI_ACLK clock cycle when both
 -- S_AXI_AWVALID and S_AXI_WVALID are asserted. axi_awready is
 -- de-asserted when reset is low.
 process (S_AXI_ACLK)
 begin
  if rising_edge(S_AXI_ACLK) then 
    if S_AXI_ARESETN = '0' then
      axi_awready <= '0';
    else
      if (axi_awready = '0' and S_AXI_AWVALID = '1' and S_AXI_WVALID = '1') then
        -- slave is ready to accept write address when
        -- there is a valid write address and write data
        -- on the write address and data bus. This design 
        -- expects no outstanding transactions. 
        axi_awready <= '1';
      else
        axi_awready <= '0';
      end if;
    end if;
  end if;
 end process;
 -- Implement axi_awaddr latching
 -- This process is used to latch the address when both 
 -- S_AXI_AWVALID and S_AXI_WVALID are valid. 
 process (S_AXI_ACLK)
 begin
  if rising_edge(S_AXI_ACLK) then 
    if S_AXI_ARESETN = '0' then
      axi_awaddr <= (others => '0');
    else
      if (axi_awready = '0' and S_AXI_AWVALID = '1' and S_AXI_WVALID = '1') then
        -- Write Address latching
        axi_awaddr <= S_AXI_AWADDR;
      end if;
    end if;
  end if;                   
 end process; 
 -- Implement axi_wready generation
 -- axi_wready is asserted for one S_AXI_ACLK clock cycle when both
 -- S_AXI_AWVALID and S_AXI_WVALID are asserted. axi_wready is 
 -- de-asserted when reset is low. 
 process (S_AXI_ACLK)
 begin
  if rising_edge(S_AXI_ACLK) then 
    if S_AXI_ARESETN = '0' then
      axi_wready <= '0';
    else
      if (axi_wready = '0' and S_AXI_WVALID = '1' and S_AXI_AWVALID = '1') then
          -- slave is ready to accept write data when 
          -- there is a valid write address and write data
          -- on the write address and data bus. This design 
          -- expects no outstanding transactions.           
          axi_wready <= '1';
      else
        axi_wready <= '0';
      end if;
    end if;
  end if;
 end process; 
 -- Implement memory mapped register select and write logic generation
 -- The write data is accepted and written to memory mapped registers when
 -- axi_awready, S_AXI_WVALID, axi_wready and S_AXI_WVALID are asserted. Write strobes are used to
 -- select byte enables of slave registers while writing.
 -- These registers are cleared when reset (active low) is applied.
 -- Slave register write enable is asserted when valid address and data are available
 -- and the slave is ready to accept the write address and write data.
 slv_reg_wren <= axi_wready and S_AXI_WVALID and axi_awready and S_AXI_AWVALID ;
 process (S_AXI_ACLK)
 variable loc_addr :std_logic_vector(OPT_MEM_ADDR_BITS downto 0); 
 begin
  if rising_edge(S_AXI_ACLK) then 
    if S_AXI_ARESETN = '0' then
      slv_reg0 <= (others => '0');
      slv_reg1 <= (others => '0');
      slv_reg2 <= (others => '0');
      slv_reg3 <= (others => '0');
      slv_reg4 <= (others => '0');
    else
      loc_addr := axi_awaddr(ADDR_LSB + OPT_MEM_ADDR_BITS downto ADDR_LSB);
      if (slv_reg_wren = '1') then
        case loc_addr is
          when b"000" =>
            for byte_index in 0 to (C_S_AXI_DATA_WIDTH/8-1) loop
              if ( S_AXI_WSTRB(byte_index) = '1' ) then
                -- Respective byte enables are asserted as per write strobes                   
                -- slave registor 0
                slv_reg0(byte_index*8+7 downto byte_index*8) <= S_AXI_WDATA(byte_index*8+7 downto byte_index*8);
              end if;
            end loop;
          when b"001" =>
            for byte_index in 0 to (C_S_AXI_DATA_WIDTH/8-1) loop
              if ( S_AXI_WSTRB(byte_index) = '1' ) then
                -- Respective byte enables are asserted as per write strobes                   
                -- slave registor 1
                slv_reg1(byte_index*8+7 downto byte_index*8) <= S_AXI_WDATA(byte_index*8+7 downto byte_index*8);
              end if;
            end loop;
          when b"010" =>
            for byte_index in 0 to (C_S_AXI_DATA_WIDTH/8-1) loop
              if ( S_AXI_WSTRB(byte_index) = '1' ) then
                -- Respective byte enables are asserted as per write strobes                   
                -- slave registor 2
                slv_reg2(byte_index*8+7 downto byte_index*8) <= S_AXI_WDATA(byte_index*8+7 downto byte_index*8);
              end if;
            end loop;
          when b"011" =>
            for byte_index in 0 to (C_S_AXI_DATA_WIDTH/8-1) loop
              if ( S_AXI_WSTRB(byte_index) = '1' ) then
                -- Respective byte enables are asserted as per write strobes                   
                -- slave registor 3
                slv_reg3(byte_index*8+7 downto byte_index*8) <= S_AXI_WDATA(byte_index*8+7 downto byte_index*8);
              end if;
            end loop;
          when b"100" =>
            for byte_index in 0 to (C_S_AXI_DATA_WIDTH/8-1) loop
              if ( S_AXI_WSTRB(byte_index) = '1' ) then
                -- Respective byte enables are asserted as per write strobes                   
                -- slave registor 4
                slv_reg4(byte_index*8+7 downto byte_index*8) <= S_AXI_WDATA(byte_index*8+7 downto byte_index*8);
              end if;
            end loop;
          when others =>
            slv_reg0 <= slv_reg0;
            slv_reg1 <= slv_reg1;
            slv_reg2 <= slv_reg2;
            slv_reg3 <= slv_reg3;
            slv_reg4 <= slv_reg4;
        end case;
      end if;
    end if;
  end if;                   
 end process; 
 -- Implement write response logic generation
 -- The write response and response valid signals are asserted by the slave 
 -- when axi_wready, S_AXI_WVALID, axi_wready and S_AXI_WVALID are asserted.  
 -- This marks the acceptance of address and indicates the status of 
 -- write transaction.
 process (S_AXI_ACLK)
 begin
  if rising_edge(S_AXI_ACLK) then 
    if S_AXI_ARESETN = '0' then
      axi_bvalid  <= '0';
      axi_bresp   <= "00"; --need to work more on the responses
    else
      if (axi_awready = '1' and S_AXI_AWVALID = '1' and axi_wready = '1' and S_AXI_WVALID = '1' and axi_bvalid = '0'  ) then
        axi_bvalid <= '1';
        axi_bresp  <= "00"; 
      elsif (S_AXI_BREADY = '1' and axi_bvalid = '1') then   --check if bready is asserted while bvalid is high)
        axi_bvalid <= '0';                                 -- (there is a possibility that bready is always asserted high)
      end if;
    end if;
  end if;                   
 end process; 
 -- Implement axi_arready generation
 -- axi_arready is asserted for one S_AXI_ACLK clock cycle when
 -- S_AXI_ARVALID is asserted. axi_awready is 
 -- de-asserted when reset (active low) is asserted. 
 -- The read address is also latched when S_AXI_ARVALID is 
 -- asserted. axi_araddr is reset to zero on reset assertion.
 process (S_AXI_ACLK)
 begin
  if rising_edge(S_AXI_ACLK) then 
    if S_AXI_ARESETN = '0' then
      axi_arready <= '0';
      axi_araddr  <= (others => '1');
    else
      if (axi_arready = '0' and S_AXI_ARVALID = '1') then
        -- indicates that the slave has acceped the valid read address
        axi_arready <= '1';
        -- Read Address latching 
        axi_araddr  <= S_AXI_ARADDR;           
      else
        axi_arready <= '0';
      end if;
    end if;
  end if;                   
 end process; 
 -- Implement axi_arvalid generation
 -- axi_rvalid is asserted for one S_AXI_ACLK clock cycle when both 
 -- S_AXI_ARVALID and axi_arready are asserted. The slave registers 
 -- data are available on the axi_rdata bus at this instance. The 
 -- assertion of axi_rvalid marks the validity of read data on the 
 -- bus and axi_rresp indicates the status of read transaction.axi_rvalid 
 -- is deasserted on reset (active low). axi_rresp and axi_rdata are 
 -- cleared to zero on reset (active low).  
 process (S_AXI_ACLK)
 begin
  if rising_edge(S_AXI_ACLK) then
    if S_AXI_ARESETN = '0' then
      axi_rvalid <= '0';
      axi_rresp  <= "00";
    else
      if (axi_arready = '1' and S_AXI_ARVALID = '1' and axi_rvalid = '0') then
        -- Valid read data is available at the read data bus
        axi_rvalid <= '1';
        axi_rresp  <= "00"; -- 'OKAY' response
      elsif (axi_rvalid = '1' and S_AXI_RREADY = '1') then
        -- Read data is accepted by the master
        axi_rvalid <= '0';
      end if;            
    end if;
  end if;
 end process;
 -- Implement memory mapped register select and read logic generation
 -- Slave register read enable is asserted when valid address is available
 -- and the slave is ready to accept the read address.
 slv_reg_rden <= axi_arready and S_AXI_ARVALID and (not axi_rvalid) ;
 process (slv_reg0, slv_reg1, slv_reg2, slv_reg3, slv_reg4, axi_araddr, S_AXI_ARESETN, slv_reg_rden)
 variable loc_addr :std_logic_vector(OPT_MEM_ADDR_BITS downto 0);
 begin
    -- Address decoding for reading registers
    loc_addr := axi_araddr(ADDR_LSB + OPT_MEM_ADDR_BITS downto ADDR_LSB);
    case loc_addr is
      when b"000" =>
        reg_data_out <= slv_reg0;
      when b"001" =>
        reg_data_out <= slv_reg1;
      when b"010" =>
        reg_data_out <= slv_reg2;
      when b"011" =>
        reg_data_out <= slv_reg3;
      when b"100" =>
        reg_data_out <= slv_reg4;
      when others =>
        reg_data_out  <= (others => '0');
    end case;
 end process; 
 -- Output register or memory read data
 process( S_AXI_ACLK ) is
 begin
  if (rising_edge (S_AXI_ACLK)) then
    if ( S_AXI_ARESETN = '0' ) then
      axi_rdata  <= (others => '0');
    else
      if (slv_reg_rden = '1') then
        -- When there is a valid read address (S_AXI_ARVALID) with 
        -- acceptance of read address by the slave (axi_arready), 
        -- output the read dada 
        -- Read address mux
          axi_rdata <= reg_data_out;     -- register read data
      end if;   
    end if;
  end if;
 end process;
 -- Add user logic here
 -- User logic ends
end arch_imp;
```

The entire BASYS_7_seg_AXI IP core is attached for your reference. Next let’s see how we can locate and place this AXI IP core we just created, in one of our block designs!


## Resource

-   [BASYS_7_seg_AXI_1.0.zip](https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/BASYS_7_seg_AXI_1.0.zip)

---

[Previous](./27_Create-an-AXI-IP-Core-Peripheral-Step-1.md) | [Next](./29_Create-an-AXI-IP-Core-Peripheral-Step-3.md)