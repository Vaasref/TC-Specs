```
                              __
                 ____________ ____
             _______________  _________
          _________________  ______________
       ___________________  __________________                 __________________
     _____________________  ________________________        ________________________
    _______________________  ______________    _____          _______      __________
   _________________________  ________________     __    ________________    ________
  ___________________________ ____         _______      _______    ________   _______
  ____________________        _                ___        ________    _____    ______
  _________________                                        _________   _____   ______
  ________________                                         _________   _____   ______
  ________________                                       ___________   ____   ______
  ________________                                   ______________    ____  ______
  _________________                              _________________    ___   _____
   ____________________                    ______________________    __   ____
    __________________________________________________________
     _______________________________________________________
       __________________________________________________
         ____________________________________________
            ____________________________________
                 _______________________

```

# HSDP-1D High Speed Data Printer

| Item | Value | Comment |
| :--- | :--- | :--- |
| Vendor code | 0xF6956D00 | SlapDragon |
| Device ID   | 0x48534450 | HSDP-1D High Speed Data Printer |
| Device Type | 0xcff2     | Generic, nonstandard output device |
| Version     | 0x0144     | |

## Description

The HSDP line is all about printing the data you need as fast as possible.

The HSDP line offers you the best way to store your data, in terms of cost
efficiency.

Paper, power and data is all you need to run a HSDP. Its clever design combining
the speed of the line matrix and the ink-free design of a spark printer will
really cut the cost of your radio survey by orders of magnitude.

The HSDP line is also very quiet - SlapDragon guarantees less than 95dB of noise
at full speed!

The HSDP-1D is a Data version of the model 1. Equipped with cutting edge memory
chips and buses, this printer will be able to log everything you throw at it.
It is built to print large amounts of raw data in a smaller amount of time.

## Paper Format

The paper used by the HSDP-1D is a continuous reel without natural "pages".

Interrupt `2` can order the printer to cut the page after any line.

Lines hold a maximum of 80 printed characters.

## Speed

The HSPD-1D prints 5 lines per second. (The content of the line doesn't matter,
blank lines are not "skipped".)

This works out to 300 lines per minute, or 5 pages per minute at 60 lines per
page. (The HSPD-1D doesn't use cut pages, this is just for comparison with
inferior printers from other manufacturers.)


## Printing Modes

The HSDP line is built around modes. The model 1D has three modes:

| ID   | Mode Name | Content of one line       | Line Size |
| :--- | :---      | :---                      | :---      |
| 0    | Text Mode | 80 ASCII characters       | 80 words  |
| 1    | Data Mode | 16 words                  | 16 words  |
| 2    | Hex  Mode | 8 words                   |  8 words  |

When ordered to print a line (see the interrupts below), the HSDP will read 80
words (mode 0), 16 words (mode 1) or 8 words (mode 2) from the DCPU memory, and
print a line containing that data.

### Mode 0

Mode 0 prints standard ASCII text. Only the lower 7 bits of each word are
actually respected when printing. Control characters (including newlines,
carriage returns and tabs) are treated as simple characters and thus blanks
with the default font.

### Mode 1

Mode 1 prints 16 words formatted as hex with spaces between:

```
0000 1111 2222 3333 4444 5555 6666 7777  8888 9999 aaaa bbbb cccc dddd eeee ffff
```

### Mode 2

Mode 2 prints 8 words formatted as hex, in a "hex dump" style:

```
80b0:  0000 1111 2222 3333  4444 5555 6666 7777    01234567
```

Where the rightmost block of text is the ASCII equivalents (where defined) or
`.` if non-printing.

As with Mode 0 ASCII text, the upper 9 bits of each word are ignored for ASCII
purposes.


## Interrupt Behavior

- **0x0000: Set Mode** - Reads register `B` and sets the mode to  `B`.
  (Undefined modes will result in undefined behavior.)
- **0x0001: Get Mode** - Sets register `A` to the current mode.
- **0x0002: Buffer status** - Sets `A` to the number of lines worth of data
  currently buffered. If this returns 0, then calls to `3`, `4` or `5` won't
  block (unless you tell `5` to print more than will fit in the buffer). Note
  that the number of lines depends on the mode.
- **0x0003: Cut Page** - Cuts the text at this point. Blocks until all buffered
  data is printed (see below on buffering).
- **0x0004: Print single line** - Reads `B` and prints a full line (according
  to the current mode) from the DCPU memory starting at `B`.
- **0x0005: Print multiple lines** - Reads `B` and `C`, and prints `C` full
  lines (according to the current mode) from memory starting at `B`.
- **0x0006: Full dump** - Halts the DCPU and prints the entire memory in the
  current mode. Note that this takes quite a while!
    - Mode 0: 410 seconds (3 minutes, 25 seconds)
    - Mode 1: 2048 seconds (17 minutes, 4 seconds)
    - Mode 2: 4906 seconds (34 minutes, 8 seconds)

- **0xFFFF: Reset** - The standard reset interrupt. Resets the mode to 0, and
  cuts the current page, if any.

## Buffers and Speed

The HSDP-1D has an internal buffer that allows it to quickly copy memory from
the DCPU and let the DCPU continue while printing is still in progress. That
memory can hold 640 words of data.

640 words is enough for 8 lines in mode 0, 40 lines in mode 1, and 80 lines in
mode 2.

If ordered to print more than will fit in this buffer, the HSPD-1D will halt the
DCPU until all data to be printed is either on paper or in the buffer, and then
the DCPU proceeds.

When interrupt orders the page to be cut, and the buffer is nonempty, the
DCPU is halted until the buffered data is completely printed, then the page is
cut and control is returned to the DCPU.
