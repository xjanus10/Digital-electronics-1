
# Lab 4: Segment
### Github link: https://github.com/xjanus10/Digital-electronics-1/blob/main/Labs/04-segment/README.md

## 1) Preparation tasks

### Table with connection of 7-segment displays on Nexys A7 board

| **SW** | **AN** | **SW PIN** | **AN PIN** | **7-seg LED** | **7-seg PIN** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| **SW0** | AN7 | J15 | U13 | CA | T10 |
| **SW1** | AN6 | L16 | K2 | CB | R10 |
| **SW2** | AN5 | M13 | T14 | CC | K16 |
| **SW3** | AN4 | R15 | P14 | CD | K13 |
| **SW4** | AN3 | R17 | J14 | CE | P15 |
| **SW5** | AN2 | T18 | T9 | CF | T11 |
| **SW6** | AN1 | U18 | J18 | CG | L18 |
| **SW7** | AN0 | R13 | J17 | DP | H15 |

### Decoder truth table for common anode 7-segment display
| **Hex** | **Inputs** | **A** | **B** | **C** | **D** | **E** | **F** | **G** |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0000 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| 1 | 0001 | 1 | 0 | 0 | 1 | 1 | 1 | 1 |
| 2 | 0010 | 0 | 0 | 1 | 0 | 0 | 1 | 0 |
| 3 | 0011 | 0 | 0 | 0 | 0 | 1 | 1 | 0 |
| 4 | 0100 | 1 | 0 | 0 | 1 | 1 | 0 | 0 |
| 5 | 0101 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 6 | 0110 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 7 | 0111 | 0 | 0 | 0 | 1 | 1 | 1 | 1 |
| 8 | 1000 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 9 | 1001 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| A | 1010 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| b | 1011 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| C | 1100 | 0 | 1 | 1 | 0 | 0 | 0 | 1 |
| d | 1101 | 1 | 0 | 0 | 0 | 0 | 1 | 0 |
| E | 1110 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| F | 1111 | 0 | 1 | 1 | 1 | 0 | 0 | 0 |



## 2) Seven-segment display decoder
### hex_7seg.vhd

```vhdl
architecture Behavioral of hex_7seg is
begin
    p_7seg_decoder : process(hex_i)
    begin
        case hex_i is
            when "0000" =>
                seg_o <= "0000001";  
            when "0001" =>
                seg_o <= "1001111";  
            when "0010" =>
                seg_o <= "0010010";  
            when "0011" =>
                seg_o <= "0000110";  
            when "0100" =>
                seg_o <= "1001100";  
            when "0101" =>
                seg_o <= "0100100"; 
            when "0110" =>
                seg_o <= "0100000";  
            when "0111" =>
                seg_o <= "0001111";  
            when "1000" =>
                seg_o <= "0000000";  
            when "1001" =>
                seg_o <= "0000100";  
            when "1010" =>
                seg_o <= "0001000";  
            when "1011" =>
                seg_o <= "1100000";  
            when "1100" =>
                seg_o <= "0110001";  
            when "1101" =>
                seg_o <= "1000010";  
            when "1110" =>
                seg_o <= "0110000";  
            when others =>
                seg_o <= "0111000";  
        end case;
    end process p_7seg_decoder;
```

### tb_hex_7seg.vhd

```vhdl
p_7seg_decoder : process
    begin
        report "Stimulus process started" severity note;
        s_hex <= "0000"; wait for 100 ns;
        s_hex <= "0001"; wait for 100 ns;
        s_hex <= "0010"; wait for 100 ns;
        s_hex <= "0011"; wait for 100 ns;
        s_hex <= "0100"; wait for 100 ns;
        s_hex <= "0101"; wait for 100 ns;
        s_hex <= "0110"; wait for 100 ns;
        s_hex <= "0111"; wait for 100 ns;
        s_hex <= "1000"; wait for 100 ns;
        s_hex <= "1001"; wait for 100 ns;
        s_hex <= "1010"; wait for 100 ns;
        s_hex <= "1011"; wait for 100 ns;
        s_hex <= "1100"; wait for 100 ns;
        s_hex <= "1101"; wait for 100 ns;
        s_hex <= "1110"; wait for 100 ns;
        s_hex <= "1111"; 
        report "Stimulus process finished" severity note; 
        wait;
    end process p_7seg_decoder;
```

### Simulated time waveforms
![screen](images/1.png)

### top.vhd
```vhdl
hex2seg : entity work.hex_7seg
        port map(
            hex_i    => SW, 
            seg_o(6) => CA,  
            seg_o(5) => CB,
            seg_o(4) => CC,
            seg_o(3) => CD,
            seg_o(2) => CE,
            seg_o(1) => CF,
            seg_o(0) => CG
        );
```

## 3) LED indicators

### Truth table for LEDs
| **Hex** | **Inputs** | **LED4** | **LED5** | **LED6** | **LED7** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0000 | 1 | 0 | 0 | 0 |
| 1 | 0001 | 0 | 0 | 1 | 1 |
| 2 | 0010 | 0 | 0 | 0 | 1 |
| 3 | 0011 | 0 | 0 | 1 | 0 |
| 4 | 0100 | 0 | 0 | 0 | 1 |
| 5 | 0101 | 0 | 0 | 1 | 0 |
| 6 | 0110 | 0 | 0 | 0 | 0 |
| 7 | 0111 | 0 | 0 | 1 | 0 |
| 8 | 1000 | 0 | 0 | 0 | 1 |
| 9 | 1001 | 0 | 0 | 1 | 0 |
| A | 1010 | 0 | 1 | 0 | 0 |
| b | 1011 | 0 | 1 | 1 | 0 |
| C | 1100 | 0 | 1 | 0 | 0 |
| d | 1101 | 0 | 1 | 1 | 0 |
| E | 1110 | 0 | 1 | 0 | 0 |
| F | 1111 | 0 | 1 | 1 | 0 |

### VHDL code for LEDs
```vhdl
LED(3 downto 0) <= SW;
LED (4) <=  '1' when (SW = "0000") else '0'; 
LED (5) <=  '1' when (SW > "1001") else '0'; 
LED (6) <=  '1' when (SW(0) = '1') else '0'; 
LED (7) <=  '1' when (SW = "0001") else 
            '1' when (SW = "0010") else
            '1' when (SW = "0100") else
            '1' when (SW = "1000") else '0';   
```
### Simulated time waveforms
![screen](images/2.png)
