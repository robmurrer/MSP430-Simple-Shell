# User Manual

The Simple Shell for MSP430 UART (SSMU) provides a serial interface to the Texas Instruments (TI) MSP-EXP430G2 Experimenter Board.  This interface allows the memory to be displayed or modified and the Arithmetic Logic Unit's (ALU) addition or subtraction can be executed.

## Requirements
* Basic computer knowledge of opening and running programs
* TI MSP-EXP430G2 Experimenter Board (TIEB) with the SSMU program loaded
* Personal Computer with free USB port
* Terminal program with the following configuration: 9600 baud, 8 Data bits, 1 Stop bit, No Parity.
* USB cable with micro-usb connection for connection with the TIEB

## Getting Started
1. Connect the TIEB to the computer with the micro-usb cable.
2. Determine which Com port is being used by the TIEB.  In MS Windows this is done by going to the Control Panel then to Device Manager and then to Ports.  Look for "MSP430 Application UART (COMx) where 'x' would be 1-6 normally.
3. Open up Terminal program and setup a connection to the TIEB with the Com Port determined in previous step and the configuration of 9600 baud, 8 Data bits, 1 Stop bit, No Parity
4. Type `HS FFFF FFFF` the result should be `0000` along with the status bits underneath.
5. The command prompt of `> ` should be displayed and is waiting on the next command.

## Addition and Subtraction
The SSMU can be used as a simple calculator performing both addition and subtraction.  The commands `HA` and `HS` are used for addition and subtraction.  These commands take two 4 digit arguments.  The arguments must be valid hexadecimal numbers such as `0FEE` or `AADD`.

The results of the addition and subtraction are followed by a line of additional information that can aid in making sense of the answer returned. The flags are as follows: 

    V=x N=x Z=x C=x

The value of x can be `0` or `1`. Zero means the flag is not set and One means it is set.

* `V` is for overflow. The sign of the value returned is incorrect. (ie: positive + positive = negative) 
* `N` is for negative. The value returned is a negative number.
* `Z` is for zero. The value returned is zero.
* `C` is for carry. During the operation the carry flag was set.

### Examples
    > HA 0000 0001 = 0001
    V=0 N=0 Z=0 C=0

    > HA FFFF FFFF = FFFE
    V=0 N=1 Z=0 C=1

    > HS 2525 3535 = EFF0
    V=0 N=1 Z=0 C=0

    > HS FFFF FFFF = 0000
    V=0 N=0 Z=1 C=1


## Displaying Values in Memory
The SSMU can be used to view the data in memory.  The data is displayed 8 words per line and displayed with the address and data along with the ASCII representation--if it is printable.

The command to display memory is `D XXXX YYYY` where `XXXX` is the hexadecimal start address and `YYYY` is the hexadecimal end address.  The display is always 8 words wide and the end address is non-inclusive.

### Example
    > D
    0200 0280
    0200 0203 0202 0910 0204 C4A6 0206 0072 0208 7480 020A 0420 020C 1614 020E 7600 ......r..t.....v
    0210 0594 0212 8104 0214 D451 0216 1020 0218 81E1 021A 1B0E 021C B888 021E 0C00 ....Q...........
    0220 02B8 0222 0404 0224 0964 0226 6042 0228 0604 022A 8124 022C 4825 022E 2001 ....d.B`..$.%H..
    0230 1096 0232 1102 0234 14C4 0236 1000 0238 90A1 023A 2082 023C 86A1 023E 0490 ................
    0240 BEFF 0242 FFF7 0244 7E76 0246 BFFF 0248 5BC4 024A 1FF3 024C DD8D 024E 8D5E ....v~...[....^.
    0250 7BEB 0252 5ADD 0254 78FC 0256 72BE 0258 3FF7 025A DFFD 025C FDD7 025E 5C9D .{.Z.x.r.?.....\
    0260 DFFE 0262 7FFE 0264 7F16 0266 F955 0268 EDFE 026A FEFD 026C 8FF7 026E F5BF ......U.........
    0270 FCE7 0272 DFBE 0274 C9FD 0276 FFEF 0278 7E32 027A C7FF 027C FFAA 027E 9DD1 ........2~...... 


## Modifying Contents of Memory
The SSMU can be used to modify the data in memory.  To enter this mode of operation the user must type `M XXXX` where `XXXX` is the address to be modified.  Once the user is in this mode the SSMU will display `AAAA VVVV` where `AAAA` is the address and `VVVV` is the current hexadecimal representation of the data in in that address.  The user is now prompted to enter in a new data value to be inserted into that location.

To exit the modify memory mode the user must type enter in a `space`.  If the user enters in a valid hex value the SSMU will insert the data and then go to the next word in memory to be modified.

To navigate the memory the user may enter in an `N` or a `P` at any time in the modify memory mode.

### Commands
* `space` : exit modify memory mode
* `N` : go backwards to the previous word
* `P` : go forwards to the next word in memory

### Examples

    > M 0300
    0300 084A FFFF
    0302 4918 FF11
    0304 10C0 N
    0302 FF11 N
    0300 FFFF

    > M 0300
    0300 FFFF 22P
    0302 FF11 33N
    0300 FFFF 3333
    0302 FF11 N
    0300 3333
   

# Test procedure
To validate that the Simple Shell for MSP430 UART (SSMU) for the Texas Instruments MSP-EXP430G2 Experimenter Board (TIEB) works correctly the following can be used.

## Requirements
* Personal Computer with Microsoft Windows 7 and a free USB port
* Code Composer Studio (CCS) 5.3
* Project files and source for SSMU
* TIEB and micro-usb cable 
* Putty 0.63 Terminal Program

## Setup
1. Turn on computer and wait for operating system to fully load.
2. Plug USB cable into computer and TIEB.
3. Start the CCS application from the `Start Menu`.
4. From the `File` menu select `Import...`
5. Select the folder that contains the Project files for the SSMU.
6. Select `Code Composer Studio` then `Existing CCS Eclipse Projects`.
7. Browse for project folder and click finish.
8. From the `View` menu select `Project Explorer`.
9. Click the project and it will become the active project.
10. Press the `F11` Key and the project will compile and load.
11. Wait until the compilation is finished and press the `F8` key.
12. Press the `Windows` key and the `Pause/Break` key at the same time.
13. Click on the `Device Manager` link.
14. Select the `Ports` item.
15. Look for the `MSP430 Application UART` and record the Com Port.
16 Start the Putty Application.
17. In the Putty Settings set the `connection type` to `serial`.
18. Enter the `Serial line` as `COMx` where x is the value found in previously.
19. Set `Speed` to `9600`.
20. The defaults in Putty are adequate: `Data bits`: 8, `Stop bit`: 1, `Parity`: None.
21. The SSMU is now ready to be tested.


## Basic Addition
### Input

    HA 0300 0500 

### Output

    > HA 0300 0500 = 0800
    V=0 N=0 Z=0 C=0

## Basic Subtraction
### Input

    HA 0500 0300 

### Output

    > HS 0500 0300 = 0200
    V=0 N=0 Z=0 C=1

## Zero Flag
### Input
    HS 0300 0300
### Output
    > HS 0300 0300 = 0000
    V=0 N=0 Z=1 C=1

## Negative Flag
### Input
    HS 0300 0400
### Output
    > HS 0300 0400 = FF00
    V=0 N=1 Z=0 C=0

## Overflow Flag
### Input
    HS 9999 9999
### Output
    > HA 9999 9999 = 3332
    V=1 N=0 Z=0 C=1

## Display Memory
### Input
    D 0200 0200 
### Output
    > D
    0200 0200
    0200 0000 0202 6548 0204 6C6C 0206 206F 0208 2020 020A 2020 020C 2020 020E 2020 ..Hello......

## Display 16 Words of Memory
### Input
    D 0200 021F 
### Output
    > D
    0200 021F
    0200 0000 0202 6548 0204 6C6C 0206 206F 0208 2020 020A 2020 020C 2020 020E 2020 ..Hello......
    0210 07DC 0212 8114 0214 D453 0216 12A0 0218 D1E1 021A 1B0E 021C B8A8 021E 0C00 ....S........

## Modify Memory Backwards/Forwards Skip
### Input
    M 0208 N P P <space>
### Output
    > M 0208
    0208 2020 N
    0206 206F P
    0208 2020 P
    020A 2020
    >

## Modify Memory Insert
### Input
    M 0208 2121 <space> D 0200 0200
### Output
    > M 0208
    0208 2020 2121
    020A 2020
    > D
    0200 0200
    0200 0000 0202 6548 0204 6C6C 0206 206F 0208 2020 020A 2020 020C 2020 020E 2020 ..Hello!!....


