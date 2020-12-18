***************************
DDR3 Data Structure Example
***************************

To understand how the data structure of a DDR3 readout looks like.

Features that are equal to the normal acquisition data format:
* A single word is 16 bit
* There is a header made of 8 single words, equal to normal acquisition
* There is a trailer made of 8 single words, equal to normal acquisition
* In between header and trailer there is the payload

Features that differ form normal acquisition data format:
* All channels are included in a single file (header tells you the channel)
* Data is arranged in a different way (see the following)

Let's pretend that this is the header:

0xH1
0xH2
0xH3
0xH4
0xH5
0xH6
0xH7
0xH8

this is the trailer:

0xT1
0xT2
0xT3
0xT4
0xT5
0xT6
0xT7
0xT8

and this is the payload (with a trigger window of 24 data samples)

0xD1
0xD2
0xD3
.
.
.
0xD23
0xD24


The acquisition file will look like this:


0xT1

0xT2

0xT3

0xT4

0xT5

0xT6

0xT7

0xT8

0xD17

0xD18

0xD19

0xD20

0xD21

0xD22

0xD23

0xD24

0xD9

0xD10

0xD11

0xD12

0xD13

0xD14

0xD15

0xD16

0xD1

0xD2

0xD3

0xD4

0xD5

0xD6

0xD7

0xD8

0xH1

0xH2

0xH3

0xH4

0xH5

0xH6

0xH7

0xH8

And then the trailer of the PREVIOUS data begins..

So the order is inverted in groups of 128 bits, but inside the single 128 bit chunk, the order is normal.
