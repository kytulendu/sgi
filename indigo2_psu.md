SGI Indigo 2 PSU
================

ZYTEC 385W (P/N 060-0021-001 REV C)
-----------------------------------

Specs

    ZYTEC 385W  SGI P/N:060-0021-001 REV C

    +5.0V/40.0A
    +3.5V/48.0A
    +3.5V/12.0A
    +12.0V/4.25A
    -12.0V/0.5A
    -5.0V/0.8A
    +5.0V AUX/0.1A

Capacitor list

Include what type of capacitors I use.

![HV board capacitors](./resources/ZYTEC_385W_060-0021-001_RevC_HV.jpg)

HV board capacitor list

    1uF 50v         x1      Rubycon YXF
    10uF 35v        x1      ELNA RJJ
    47uF 35v        x1      ELNA RJB
    100uF 25v       x1      Panasonic FC, 100uF 35v
    330uF 35v       x1      Nippon Chemi-Con LXZ
    470uF 35v       x2      Nichicon HW
    1800uF 200v     x2      original one is Marcon DUF series, didn't change

Note:
- I usually didn't change bulk caps if it measure to be okey.
- Capacitors on HV board can use 105°C general purpose, but preferably long life type.

![LV board capacitors](./resources/ZYTEC_385W_060-0021-001_RevC_LV.jpg)

LV board capacitor list

    47uF 25v        x1      ELNA RJF
    47uF 35v        x2      ELNA RJB
    330uF 6.3v      x3      original one is Marcon CFM series (polymer capacitor), didn't change
    470uF 35v       x2      Nichicon HW
    2200uF 16v      x1      Nippon Chemi-Con KZE
    3300uF 10v      x1      SAMXON GK (pulled from broken Dell or HP PSU lol), will change to Chemi-Con KZE at later date
    3300uF 16v      x1      ELNA RJH, 3300uF 16v the pin width is too narrow but was able to fit.
                            Use 3900uF or Nichicon PW or Panasonic FC for 16mm width, which will fit the PCB hole.
    6800uF 6.3v     x6      Nichicon PM (Prefer PW series as it is longer life version)

PSU fan is Panaflo FBA09A12V

More information on SGI Indigo 2 power supply
---------------------------------------------
- [Indigo 2 Power Supply Basics](https://forums.sgi.sh/index.php?threads/indigo-2-power-supply-basics.111/)
