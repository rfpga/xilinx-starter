# 22. Introduction to IP Cores

Welcome to the Introduction to IP Cores lecture. In this lecture I am going to introduce and explain what an IP core is. Almost all advanced hardware designs targeting a Xilinx FPGA will be using an IP core of some sort. Vivado comes with free to use IP cores, you can also purchase IP cores from Xilinx and 3rd part vendors. When you are working with very complex designs IP cores are extremely useful in allowing you to quickly test and implement your actual design.

**What is an IP Core?**

IP stands for Intellectual Property, which is this case is the underlying HDL design. The IP core represents the top level HDL design where the entity is shown as the inputs and outputs of the IP core, and the architecture is symbolized as a block. Let’s look at a design that takes in a 4-digit hexadecimal value and outputs the corresponding 7 segment display value.


```
-- Hex to 7-segment conversion
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity Hex_to_7_Seg is
port (
 seven_seg : out std_logic_vector(6 downto 0);
 hex : in std_logic_vector(3 downto 0));
end hex_to_7_seg;
architecture behavior of Hex_to_7_Seg is
  
signal seg_out : std_logic_vector(6 downto 0);
begin  
 --  7 segs are active low
 seven_seg <= not seg_out;  
 seg_proc : process(hex)
 begin 
 case hex is
 when x"0" => seg_out <= "0111111";
 when x"1" => seg_out <= "0000110";
 when x"2" => seg_out <= "1011011";
 when x"3" => seg_out <= "1001111";
 when x"4" => seg_out <= "1100110";
 when x"5" => seg_out <= "1101101";
 when x"6" => seg_out <= "1111101";
 when x"7" => seg_out <= "0000111";
 when x"8" => seg_out <= "1111111";
 when x"9" => seg_out <= "1101111";
 when x"A" => seg_out <= "1110111";
 when x"B" => seg_out <= "1111100";
 when x"C" => seg_out <= "0111001";
 when x"D" => seg_out <= "1011110";
 when x"E" => seg_out <= "1111001";
 when x"F" => seg_out <= "1110001";
 when others =>
 seg_out <= (others => 'X');
 end case;
 end process seg_proc; 
end behavior;
```

Now if we want to represent the Hex_to_7_Seg design as an IP core we are only looking at the inputs and outputs. Figure 1 shows the relationship between the HDL design file and the IP core.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/hex_7_ip_core_with_HDL.png" width="90%" > 
</p> 


Let’s take a look at another example, a design that takes in 4 – 7 segment display inputs, decimal place indicators and a clock input. This design then is used to control the toggle rates and display all 4 digits of the 7 segment display located on the BASYS 2 or BASYS 3 development board.

```
-------------------------------------------------------------------------------
-- FILE: BASYS_7_seg.vhd
--
-- DESCRIPTION: This design is used to control the 7 segment display on  
-- the BASYS 2 or 3 FPGA development board.
--
-- ENGINEER: Jordan Christman
-------------------------------------------------------------------------------
-- Libraries
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.numeric_std.all;
use IEEE.STD_LOGIC_UNSIGNED.ALL;
-- Entity
entity BASYS_7_seg is
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
end BASYS_7_seg;
architecture behavior of BASYS_7_seg is
-------------------------------------------------------------------------------
-- Constants
-------------------------------------------------------------------------------
-- 7 Seg display counter mulitply by 4 to take the 4 segments in account
constant maxcount : integer := (input_clk_freq / refresh_rate) * 4; 
-------------------------------------------------------------------------------
-- Signals
-------------------------------------------------------------------------------
-- A 27-bit counter is required to count to 100000000 (100 million/ 100 MHz)
-- we will be counting to 200,000 to achieve a 500Hz refresh rate 
signal counter : unsigned(26 downto 0) := to_unsigned(0, 27);
-- toggle is used to select which segment is currently being displayed
signal toggle : std_logic_vector(3 downto 0) := "0111";
-------------------------------------------------------------------------------
-- BASYS (2/3) 7 SEGMENT DISPLAY ARCHITECTURE
-------------------------------------------------------------------------------
begin
 -- Anode segment drivers
 an <= toggle;
 
 ---------------------------------------------------------------------------
 -- COUNTER PROCESS
 ---------------------------------------------------------------------------
 -- Counter to create desired multiplexed rate and shift toggle bits
 counter_proc: process(clk)
 begin
 if(rising_edge(clk)) then
 if(counter = maxcount) then
 counter <= (others => '0');
 toggle(0) <= toggle(3);
 toggle(1) <= toggle(0);
 toggle(2) <= toggle(1);
 toggle(3) <= toggle(2);
 else
 counter <= counter + 1;
 end if;
 end if;
 end process counter_proc; 
 
 ---------------------------------------------------------------------------
 -- TOGGLE PROCESS
 ---------------------------------------------------------------------------
 -- Toggle the seven segment displays by selecting the next segments display
 toggle_proc: process(toggle)
 begin
 if(toggle(0) = '0') then
 seg <= seg0;
 dp <= in_dp(0);
 elsif(toggle(1) = '0') then
 seg <= seg1;
 dp <= in_dp(1);
 elsif(toggle(2) = '0') then
 seg <= seg2;
 dp <= in_dp(2);
 else
 seg <= seg3;
 dp <= in_dp(3);
 end if;
 end process toggle_proc;
end behavior;
```

Now let’s see what the corresponding IP core would look like, note that only the entity is shown for simplicity. This IP core can be found in Figure 2.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/basys_7_seg_ip_core.png" width="90%" > 
</p> 

**Uses for an IP Core**

We can interconnect these IP cores to create our hardware designs. We can also create a repository where all of our IP cores are located so that we can re-use our designs over and over again. Instead of re-writing code for component instantiations you can now add the same component multiple times with a single button click. Then by dragging your mouse you can interconnect all of the IP cores to quickly create a complex hardware design. Figure 3 below shows an example of a hardware design utilizing multiple IP cores.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/block_design_w_ip_cores.png" width="90%" > 
</p> 


**Comparing IP Cores vs. Traditional HDL Designs**

Traditional FPGA design would include writing all the HDL designs out by hand and then writing the HDL code to connect everything. Using IP cores you would drag and drop the IP cores into your block design and then click your mouse you can then connect the IP cores together to get your design implemented as you wish. A quick comparison of both design strategies

**IP Core Advantages**

-   *Reduces Design Time through Copy & Paste Capability*
-   *Easier to Understand the Design at a High Level*
-   *Design Can Easily be Reusable for Future Designs*

**Traditional HDL Design Advantages**

-   Developer Gains Much More Knowledge of How Design Works

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/IP_cores_win.png" width="60%" > 
</p> 

Traditional HDL design is a very important skill to have, after all someone is writing the raw HDL files that are being used in an IP core. Though I would highly suggest that when you have a design or component you find yourself using a lot, create an IP core and increase your productivity!


---

[Previous](./21_Generate-Contraints-File-and-Top-Level-HDL-File.md) | [Next](./23_Using-IP-Cores.md)