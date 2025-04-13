# Sapphire Pro Modifications
---

I just couldn't use my Sapphire Pro as-is but did a lot of modifications on it.
Now, a couple of years later, I can't exactly remember what I did and why.
During reverse engineering my modification I'm writing everything down this time.

## Drivers:


### X+Y: TMC2209

I chose Lerdge v1.0 TMC2209 drivers. X and Y drivers run sensorless homing, which requires a UART connection to the driver.

In addition, the DIAG pin is required for the StallGuard output.

I modified the Lerdge TMC2209's as follows:

* Replace pullup on ENN by pulldown (yellow) 
* Disconnect ENN from Pin 1 of the driver board
* Route the DIAG pin to Pin 1 of the board
* Add pin headers for RX + TX

The pulldown enables the driver on power-up.
ENN is low-active.
PDN_UART must remain high and cannot be used for power down.
The driver can still be powered down using the UART connection.

The PDN_UART pullup is not on the driver board, since there are but on the mainboard.
Having the pullup on the driver board would parallelize the pullups and reduce resistance.

> The node address NODEADDR is selected by MS1 (bit 0) and MS2 (bit 1) in the range 0 to 3

Since MS1 and MS2 are connected to the microcontroller, they can be set in software.

In retrospect, I'm not sure if it is worth doing this modification.

![](images/lerdge-figure0.png)

### Z+E: TMC2208

With X+Y upgraded to TMC2209, I moved the TMC2208 from X+Y to Z+E.

## Mainboard

The mainboard is a MKS Robin Nano v1.2.

Changed output voltage of U1 (MP1584EN) from 5V to 3.3V by adjusting feedback resistors R3/R4.
Removed 3.3V LDO U6.
Thus, the board does not have a 5V-rail anymore.

I don't remember why I did this, but I think it was because there is actually no 5V consumer on the board.

CAUTION: J13 cannot be set anymore, that would connect V_USB (5V nom) to the 3.3V rail.

## Bed Levelling Probe

I added a 5V induction probe for bed levelling.