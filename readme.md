# Sega Saturn CD drive log

> Command clock is 50khz

> 26us between 2 byte

> START signal mark the 1st byte

> Communication seem to use SPI mode 3 

> Data are sent in LSB order


# Command List

| Value | Operation              | comment       |
| ----- | ---------------------- | ------------  |
|  0x00 | NOP                    |               |
|  0x02 | Seek ring              |               |
|  0x03 | Read toc               |               |
|  0x04 | Stop disc              |               |
|  0x05 | ??                     |               |
|  0x0A | Audio forward          | in cd player  |
|  0x0B | Audio backward         | in cd player  |
|  0x06 | Read Data              |               |
|  0x08 | Pause                  |               |
|  0x09 | Seek                   |               |

[Yabause code](https://github.com/Yabause/yabause/blob/7e38821dbac265490f115e163c523a939acda759/yabause/src/cd_drive.c#L513)

## Status list

| Value | Operation              | comment       |
| ----- | ---------------------- | ------------  |
|  0x00 | NOP                    |               |
|  0x04 | Read toc               |               |
|  0x12 | Stopped                |               |
|  0x22 | Seeking                |               |
|  0x30 | ??                     |               |
|  0x34 | Read Audio             |               |
|  0x36 | Read Data              |               |
|  0x80 | Lid Open               |               |
|  0x83 | No Disc                |               |
|  0xB2 | Seek to Ring 1         |               |
|  0xB6 | Seek to Ring 2         |               |

[Yabause code](https://github.com/Yabause/yabause/blob/7e38821dbac265490f115e163c523a939acda759/yabause/src/cd_drive.c#L83)


## Status structure
| #byte |                        | comment       |
| ----- | ---------------------- | ------------  |
|  1    | Current status         |               |
|  2    | q subcode              |               |
|  3    | track number           |               |
|  4    | index field            |               |
|  5    | minutes                |               |
|  6    | secondes               |               |
|  7    | frame                  |               |
|  8    | ??                     | 0 or 4        |
|  9    | absolute min           |               |
| 10    | absolute sec           |               |
| 11    | absolute frame         |               |
| 12    | checksum               | ~(DATA[1]...DATA[11]) |
| 13    | ??                     | Always 0 ?    |

## NOP Command

### LID OPEN - status 0x80

| #byte  |   1  |   2  |   3  |   4  |   5  |   6  |   7  |   8  |   9  |  10  |  11  |  12  |  13  |
| ------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| status | 0x80 | 0x41 | 0x01 | 0x01 | 0x29 | 0x58 | 0x45 | 0x04 | 0x30 | 0x00 | 0x45 | 0xFD | 0x00 |
| cmd    | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0xFF | 0x00 |

### LID Close - no disc - status 0x83

| #byte  |   1  |   2  |   3  |   4  |   5  |   6  |   7  |   8  |   9  |  10  |  11  |  12  |  13  |
| ------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| status | 0x83 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x7C | 0x00 |
| cmd    | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0x00 | 0xFF | 0x00 |


# Binary data - Todo

> Transmit data in serial, 24bit, 8Bit header? 16Bit data?

> Data/Audio seem to be transfered by i2s

> With 3Mhz or 6Mhz clock (cd-rom 1x/2x speed ?)

> Some clock is running at 

> PIN 13 toggle at 41hz or 88hz like LRCK

> BCLK is running at 2.1Mhz or 4Mhz

> SDATA is 24Bit long

> PIN 11 Always low

# Pinout
20 Pin drive

| Pin   | name              | comment       |
| ----- | ----------------- | ------------  |
|  1    | RESET             |               |
|  2    | GND               |               |
|  3    | CMD_STATUS_DATA   |               |
|  4    | CMD_DATA          |               |
|  5    | CMD_ENABLE        |               |
|  6    | CMD_CLOCK         |               |
|  7    | CMD_START         |               |
|  8    | GND               |               |
|  9    | 8Mhz Clk          | main clock ?  |
| 10    | GND               |               |
| 11    | Low?              | Always low?   |
| 12    | Clk?              | Not used ? Clk ?? Wrong         |
| 13    | ??                | Not used ? / L/R Channel?? |
| 14    | GND               | Could it be a setting |
| 15    | Marker?           | Not used ?    |
| 16    | LRCK ?            | i2s L/R channel |
| 17    | BCLK ?            | i2s Clock     |
| 18    | SDATA ?           | i2s Data      |
| 19    | GND               |               |
| 20    | FRONT PANEL LED   |               |

