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

# HSDP-1 High Speed Data Printer

| Item | Value | Comment |
| :--- | :--- | :--- |
| Vendor code | 0xF6956D00 | SlapDragon |
| Device ID   | 0xcFF2F552 | HSDP - High Speed Data Printer |
| Device Type | 0xcFF2     | Generic, nonstandard output device |
| Version     | 0x0100     | Model 1 |

## Description

The HSDP line is all about printing the data you need as fast as possible.

The HSDP line offers you the best way to store your data, in terms of cost
efficiency.

Paper, power and data is all you need to run a HSDP. Its clever design combining
the speed of the line matrix and the ink-free design of a spark printer will
really cut the cost of your radio survey by orders of magnitude.

The HSDP line is also very quiet - SlapDragon guarantees less than 95dB of noise
at full speed! But it works flawlessly in a vacuum environment negating most of
the noise.

Be aware that the HSDP line is also power consuming and electromagnetically
noisy.

## Paper Format

The paper used by the HSDP line is a continuous reel.

Interrupt `3` can order the printer to cut the page after any line.

Lines hold a maximum of 80 printed characters.

## Speed

The HSPD-1 prints 4 lines per second. (The content of the line doesn't matter,
blank lines are not "skipped".)

This works out to 240 lines per minute, or 4 pages per minute at 60 lines per
page. (The HSPD-1 doesn't use cut pages, this is just for
comparison with inferior printers from other manufacturers.)


## Printing Modes

While HSDP line is built around modes. The model 1 only has one fixed mode
common to every HSDP printer and compatibles : Mode 0.


When ordered to print a line (see the interrupts below), the HSDP will buffer
the line and start printing right away.

### Mode 0

Mode 0 prints standard ASCII text. Only the lower 7 bits of each word are
actually respected when printing. Control characters (including newlines,
carriage returns and tabs) are treated as simple characters and thus blanks
with the default font.

## Interrupt Behavior

- **0x0002: Buffer status** - Return 0 for ready to print and 1 for busy.
- **0x0003: Cut Page** - Cuts the text at this point or after the line being printed.
- **0x0004: Print single line** - If not busy, reads `B` and prints a full line
  (according to the current mode) from the DCPU memory starting at `B`.
- **0xFFFF: Reset** - The standard reset interrupt. Cuts the current page, if any.

## Other Models Compatibility

The HSDP line is built around a variety of models all compatible with the
model 1.
