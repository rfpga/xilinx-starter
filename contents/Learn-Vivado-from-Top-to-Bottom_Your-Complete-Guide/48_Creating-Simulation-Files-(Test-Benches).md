# 48. Creating Simulation Files (Test Benches)

Welcome to “Creating Simulation Files (Test Benches)” lecture, in this lecture we are going to discuss the format of a proper test bench as well as give a few examples. A test bench is a HDL file that is used to test your actual design. Test benches are used as a way to simulate different inputs to your design and then you can see how your design responds by looking at the generated waveform. An example waveform can be found in Figure 1.

<p align="center" >
    <img src="https://rfpga.s3.us-west-1.amazonaws.com/Learn-Vivado-from-Top-to-Bottom_Your-Complete-Guide/waveform.png" width="90%" > 
</p> 


**Test Bench Layout**

A test bench file has an entity that is completely empty – there are no inputs or outputs in a test bench. When looking at the example test benches below you should notice that all the entities are empty. Inside the test bench you want to instantiate the component you are testing, this instantiation is typically called UUT (Unit Under Test) or dev_to_test, just depending on your preference. You will then want to create processes that are used to generate a stimulus to your design, this will do things like generate a clock signal, simulate a pushbutton, or any other type of input. You may also even create a process that checks the output of your actual design and verifies if the data is correct. Inside of test benches the “report” keyword can be used to write outputs to the simulation message screen. Writing messages can be a useful way to determine if the simulation failed and were / why it failed.

**Example Test Benches**

Below are a few test bench examples you can use a reference or starting point for your next design! First we have the test bench that is used for testing a counter. This test bench has a process that is used to simulate an input clock as well as a spate process that simulates the reset signal being released.

```
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.math_real.all;
use IEEE.numeric_std.all;
use std.textio.all ;
use ieee.std_logic_textio.all ;
entity test_Counter_1 is
end;
architecture test of test_Counter_1 is
component Counter_1 is
 Generic (
 max_val : integer := 2**30;
 synch_reset : boolean := true);
 Port (
 count   : out std_logic_vector(integer(ceil(log2(real(max_val))))-1 downto 0);
 max_count : out std_logic;
 clk     : in std_logic;
 reset     : in std_logic
 );
end component;
constant max_val : integer := 10;
constant bit_depth : integer := integer(ceil(log2(real(max_val))));
constant synch_reset : boolean := true;
signal count_out : std_logic_vector(bit_depth-1 downto 0) := (others => '0');
signal max_count_out       : std_logic := '0';
signal reset_in     : std_logic := '0';
signal clk_in           : std_logic := '0';
begin
dev_to_test : Counter_1 
 generic map (
 max_val => max_val,
 synch_reset => synch_reset)
 port map (
 count => count_out,
 max_count => max_count_out,
 clk => clk_in,
 reset => reset_in);
 
clk_proc : process
  begin
      wait for 10 ns;
      clk_in <= not clk_in;
end process clk_proc;
reset_proc : process
  begin
      wait for 15 ns;
      reset_in <= '1';
end process reset_proc;
 
end test;
```

The next simulation example is used to test an adder design. This test bench loops 99,999 times and generates random numbers to verify that the output is as expected. The adder design being tested contains no clock and therefore we are creating the 99,999 random tests to verify our adder is working correctly.

```
-- testbench for 32-bit structural adder
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.numeric_std.all;
use ieee.math_real.all; -- for UNIFORM, TRUNC functions
entity test_adder_1 is
end;
architecture test of test_adder_1 is
component adder_1
 generic(bit_depth : integer := 32);
 Port (
 s : out std_logic_vector(bit_depth-1 downto 0);
 c_out : out std_logic;
 x : in std_logic_vector(bit_depth-1 downto 0);
 y : in std_logic_vector(bit_depth-1 downto 0);
 c_in : in std_logic
 );
end component;
constant bit_depth : integer := 32;
constant max_input_value : integer := 2**(bit_depth-2);
signal x : unsigned(bit_depth-1 downto 0) := (others => '0');
signal y : unsigned(bit_depth-1 downto 0) := (others => '0');
signal c_in : std_logic := '0';
signal s : unsigned(bit_depth-1 downto 0) := (others => '0');
signal c_out : std_logic := '0';
begin
 dev_to_test : adder_1
 generic map (
 bit_depth => bit_depth)
 port map (
 unsigned(s) => s,
 c_out => c_out,
 x => std_logic_vector(x),
 y => std_logic_vector(y),
 c_in => c_in);
 
 stimulus : process
 
 -- Variables for testbench
 variable ErrCnt : integer := 0;
 variable seed1, seed2 : positive; -- Seed values for random generator
 variable rand, rval : real; -- Random real number value in range 0 to 1.0
 
 begin
 for i in 0 to 99999 loop
 
 UNIFORM(seed1, seed2, rand); --generate random number
 rval := trunc(rand*real(max_input_value));
 x <= to_unsigned(integer(rval),bit_depth);
 
 UNIFORM(seed1, seed2, rand); --generate random number
 rval := trunc(rand*real(max_input_value));
 y <= to_unsigned(integer(rval),bit_depth);
 
 wait for 10 ns;
 
 if ((c_out & s) /= x+y) then
 report "The adder is broken" severity warning;
 ErrCnt := ErrCnt+1;
 end if;
 end loop;
 
 if (ErrCnt = 0) then
 report "Success! Adder test Completed.";
 else
 report "The adder is broken." severity warning;
 end if;
 end process stimulus;
end test;
```

The final simulation test bench example is the shift register; this test bench contains a process to simulate an input clock as well as a process to simulate data being fed into the shift register.

```
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.numeric_std.all;
entity test_Shift_Reg is
end;
architecture test of test_Shift_Reg is
  
component Shift_Reg
port (A: out std_logic;
      B:     out std_logic;
  C: out std_logic;
  D: out std_logic;
  data_in: in std_logic;
  reset: in std_logic;
  clk: in std_logic);
end component;
signal data_in : std_logic := '0';
signal reset : std_logic := '1';
signal clk : std_logic := '1';
signal A, B, C, D: std_logic;
begin
      
 dev_to_test:  shift_reg 
 port map(A, B, C, D, data_in, reset, clk); 
    
 clk_stimulus:  process
 begin
 wait for 10 ns;
 clk <= not clk;
 end process clk_stimulus;
 
 data_stimulus:  process
 begin
 wait for 40 ns;
 data_in <= not data_in;
 wait for 150 ns;
 end process data_stimulus;
  
end test;
```


---

[Previous](./47_How-to-Create-Your-Own-Custom-TCL-Scripts.md) | [Next](./49_Simulating-Your-Designs-in-Vivado.md)