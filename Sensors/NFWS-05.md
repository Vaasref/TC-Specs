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

# NFWS-05 Narrow Field Wide Spectrum Reception Antenna 50cm

| Item | Value | Comment |
| :--- | :--- | :--- |
| Vendor code | 0xF6956D00 | SlapDragon |
| Device ID   | 0x2FF7XXXX | NFWS Narrow Field Wide Spectrum Reception Antenna |
| Device Type | 0x2FF7     | Sensor, nonstandard output device |
| Version     | 0x0005     | Model 0.5m |

## Description

The NFWS line feature multiple size of dishes and adaptable sample capture
combined with a powerful spectrum analyzer to give you eyes in the infinity of
cosmos.

The NFWS is not focused on wave pattern analysis, its main focus is spectrum
analysis.

The NFWS line sensors comes separated from their dishes, its mounting bracket
are standards across SlapDragon's dishes of designated size and they are
compatible with multiple ends dishes.

N.B. The NFWS line is perfectly suitable for work.

## Sample an Frequency resolution

The sample technology used is fully analogic an thus the NFWS line does not
provide access to it.

For the integrated spectrum analyzer to be able to detect a frequency noise the
sample needs to be at least 3 times longer than the period.

The frequency spectrum is constrained inside 32 8bit words, the range itself is
floating.
The maximum frequency resolution of the NFWS sensors is 1Hz.

## Interrupt Behavior

- **0x000X: Start sampling** - Start sampling for the defined amount of time
    then trigger an interrupt. Only works if state is ready.
- **0x000X: Abort sampling** - Abort the current sampling if sampling.


- **0x010X: Set sample length** - Sets the sample length to B milliseconds.
- **0x010X: Set frequency spectrum start** - Set start frequency reading B and
    C (see below).
- **0x010X: Set frequency spectrum end** - Set start frequency reading B and
    C (see below).
- **0x010X: Set output map address** - Sets the output address to B.
- **0x010X: Set interrupt** - Sets the interrupt message to B.


- **0x020X: Get sample length** - Returns sample length to B in milliseconds.
- **0x020X: Get frequency spectrum start** - Returns start frequency to B and C
    (see below).
- **0x020X: Get frequency spectrum end** - Returns end frequency to B and C
    (see below).
- **0x020X: Get output map address** - Returns the output address to B.
- **0x020X: Get interrupt** - Returns the interrupt message to B.


- **0x0F01: Get state** - Return current state to B.
- **0xFFFF: Reset** - The standard reset interrupt, reset eh NFWS to its default
    non-working state.

### Frequency

The frequency are expressed in 24bit.
The 4 LSB of the second word are the 4 MSB of the expressed frequency.

|    C    |    B    |
|  :---   |  :---   |
|0000 FFFF|ffff ffff|

### States

If any non-listed state is encountered the device should be considered broken
and should only be used after servicing.

- **0xFFFF** Default
- **0x0000** Ready, can start sampling
- **0x0001** Incomplete settings
- **0x0002** Busy
- **0x0003** Error encountered, reset device

## Other Models Compatibility

The NFWS line .
