# Date : 07/05/2024

by : Don 

---------------

create verilog code for IP Block : [Johnson Counter](https://www.myhdl.org/docs/examples/jc2.html)

```verilog
module jc2 (
    goLeft,
    goRight,
    stop,
    clk,
    q
);
// A bi-directional 4-bit Johnson counter with stop control.
// 
// I/O pins:
// --------
// clk      : input free-running slow clock 
// goLeft   : input signal to shift left (active-low switch)
// goRight    : input signal to shift right (active-low switch)
// stop     : input signal to stop counting (active-low switch)
// q        : 4-bit counter output (active-low LEDs; q[0] is right-most)

input goLeft;
input goRight;
input stop;
input clk;
output [3:0] q;
reg [3:0] q;

reg [0:0] dir;
reg run;



always @(posedge clk) begin: JC2_LOGIC
    if ((goRight == 0)) begin
        dir <= 1'b0;
        run <= 1'b1;
    end
    else if ((goLeft == 0)) begin
        dir <= 1'b1;
        run <= 1'b1;
    end
    if ((stop == 0)) begin
        run <= 1'b0;
    end
    if (run) begin
        if ((dir == 1'b1)) begin
            q[4-1:1] <= q[3-1:0];
            q[0] <= (!q[3]);
        end
        else begin
            q[3-1:0] <= q[4-1:1];
            q[3] <= (!q[0]);
        end
    end
end

endmodule
```

create clock divider 

[VHDL Code for Clock Divider (Frequency Divider)](https://allaboutfpga.com/vhdl-code-for-clock-divider/)

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.numeric_std.ALL;

entity Clock_Divider is
port ( clk,reset: in std_logic;
clock_out: out std_logic);
end Clock_Divider;

architecture bhv of Clock_Divider is

signal count: integer:=1;
signal tmp : std_logic := '0';

begin

process(clk,reset)
begin
if(reset='1') then
count<=1;
tmp<='0';
elsif(clk'event and clk='1') then
count <=count+1;
if (count = 25000) then
tmp <= NOT tmp;
count <= 1;
end if;
end if;
clock_out <= tmp;
end process;

end bhv;
```
