# PXIe-148X Getting Started Example - Troubleshooting Guide
{: .no_toc }

Refer to this document to get insights and suggestions on troubleshooting performance issues, error codes, and problems with using the PXIe-148x Getting Started Examples.

> Note: This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be appliable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Error/Warnings Codes

| Error | Explanation | Possible Causes | Troubleshooting Options |
|-|-|-|-|
| -1210 | Multiple entries for the same serial channel detected in serial channel configurations array. | | |
| -1210 | Data type %s is invalid (Serial Channel Display Parameters) | | |
| -1210 | Invalid GPIO bank | | |
| -1210 | Invalid display channel | | |
| -1210 | Data type %s is invalid (Packet from FPGA) | | |
| -52012 | FIFO overflow. Transfer aborted due to a loss of data as a result of a FIFO overflow. | | |
| -52018 | Bad packet size detected on SI%d. Transfer aborted due to data corruption as a result of a packet size that did not match the expected value. |   |   |
| -200474 | Timed out while waiting for acquisition state. | | |
| -200474 | Timed out while waiting for generation state. | | |
| -304321 | Error -304321 occurred at The requested I2C transaction was unable to complete. This is usually because the requested I2C address was not acknowledged, or because the I2C slave prematurely terminated the transaction. | - Wrong script used <br />- Script syntax error <br />- Device attached to PXIe-148x module not found | TODO: insert inline link |
| -375704 |  Display channel number %d is invalid | | |

| Warning | Explanation | Possible Causes | Troubleshooting Options |
|-|-|-|-|
| 1302 | Array size mismatch for serial output channels and active lane counts. | | |
| 1302 | Array size mismatch for serial output channels and data rates. | | |

## Common Scenarios

In additional to observed error codes, some common scenarios can occur that require investigation.

| Scenario | Possible Causes | Troubleshooting Options |
|-|-|-|
| Packet timing errors detected on generation | TODO | TODO |
| VI 'hangs' after acquisition completes but eventually stops | Display too many logged packets TODO |
| Bitfile build failure | Limited resources available in FPGA | |
| I don't see my logged I2C packets | |
| I don't see my expected GPIO line change | |


TODO: Look in csi2serdesconfig source for list of unique error codes
TODO: Look for all possible -1210 error codes


## Troubleshooting Guides

### How to debug incomplete I2C transaction errors (-304321)
Some comments on general troubleshooting tips to quickly check first
1. On a 1487, there are three loopback scripts for odd channels, even channels, or both. Ensure the correct script is used for the desirced serial channel.
2. A camera may not be powered on. Double checking the Power over Coax settings are correct to power on the camera from the PXIe-148x acquisition board.
3. Script may have invalid commands - need to compare camera data sheet vs script commands
4. Others?

Some explanation on how to do a deeper dive
Wire up the script output from Run Configuration Script VI and review for clues on where the problem is occurring
   1. TODO: insert screenshot

### How to debug image display that does not show images or has a low frame rate
This may need to merge with some of the FIFO debugging?

### How to debug packet timing errors


### How do I get more channel status information from the FPGA?
- Adding the 'serial input channel status' or 'serial channel status' (for output) cluster inside the host GSE

#### TODO: Add specific subtopics on major areas of the serial channel status cluster indicators
Deep dive on each of the status indicators inside the serial channel status clusters, possible causes, etc.
 - What does the indicator mean (simple explanation)
 - What kinds of scenarios can you sniff out based on the indicator behavior


## Next Steps
- Meeting with Daniel
- Flesh out the channel status information section
- Meeting with Jared

- Review recent escalations
  -  Tyler can help get a list for devs to review?


---
### UNSORTED TODO: Open questions / techniques we may want to include
- What kind of ECU problems have we seen?
- What kind of TAP problems have we seen?
- Fifo overflow scenario that Daniel explained where reduced gen lane count on a tap board couldn't keep up with a 'burst' on the acq side (even though 'average bandwidth' should have been fine)
- Question: Should we have a Subvi that exposes the channel status more easily in the host VI?
- Adjust frame/line/pixel skip settings to reduce system pressure
- Tweaks / adjustments to various 'block sizes' used in FIFOs?
- TODO: Figure out where to put this: csi2serdesconfig.exe tool how to use it for internal purposes. - But not really a debug topic.
- Should we show people how to make host side changes to enable >2 displays?
- Frame sync usage techniques? How/when to use it.
- Tips and tricks for simulating continuous generation with a gen board
---

## Related Documents
- [PXIe-148X Getting Started Example - Generation Help](./gse-gen-help.md)
- [PXIe-148X Getting Started Example - Acquisition Help](./gse-acq-help.md)
- [PXIe-148X Getting Started Example - Tap Help](./gse-tap-help.md)
