# Power Over Coax (PoC)

## Introduction
GMSL and FPD-Link cameras can be powered over the serial link coax cable through an inductive filter that isolates the DC power from the high-speed serial data.

![PoC-System-Diagram](../../images/PoC-System-Diagram.png)

The inductive filters and power supply capacitance store energy during normal operation. The serial cable will charge up to the DC voltage of the power source such as +12 V when using A PXIe-148x's deserializer channel internal source and DC current will flow through the inductive filters. 

The GMSL and FPD-Link standards do not support hot-plug or hot-disconnect capability. Connecting or disconnecting the coax cable while the power source is energized or even when there is still energy stored in the filters can lead to hardware damage.

To prevent damage, turn off the power source and wait until the channel has discharged before disconnecting the coax cable.
## Wait for Safe to Disconnect
- description
### Setting the timeout

