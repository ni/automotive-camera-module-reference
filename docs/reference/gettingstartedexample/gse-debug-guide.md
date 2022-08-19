# PXIe-148X Getting Started Example - Troubleshooting Guide
{: .no_toc }

Refer to this document to get insights and suggestions on troubleshooting performance issues, error codes, and problems with using the PXIe-148x Getting Started Examples.

> Note: This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be appliable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Error Codes Explained

| Error Code Number | Explanation | Typical Causes | Debugging Options |
|-|-|-|-|
| -304321 | Error -304321 occurred at The requested I2C transaction was unable to complete. This is usually because the requested I2C address was not acknowledged, or because the I2C slave prematurely terminated the transaction. | - Wrong script used <br />- Script syntax error <br />- Device attached to PXIe-148x module not found | TODO: insert inline link |


Acquisition GSE
-52012 %s 
FIFO overflow.

Transfer aborted due to a loss of data as a result of a FIFO overflow.

-52018
Bad packet size detected on SI%d.

Transfer aborted due to data corruption as a result of a packet size that did not match the expected value.

-200474 occurs before acquisition. Timed out while waiting for acquisition state


All GSE
-375704  Display channel number %d is invalid
-1210    Multiple entries for the same serial channel detected in serial channel configurations array.
-1210    Data type %s is invalid (Serial Channel Display Parameters)
-1210    Invalid GPIO bank
-1210    Invalid display channel
-1210    Data type %s is invalid (Packet from FPGA)
1302     Array size mismatch for serial output channels and active lane counts.
1302     Array size mismatch for serial output channels and data rates.

TODO: Look in csi2serdesconfig source for list of unique error codes
TODO: Look for all possible -1210 error codes

Generation GSE
-200474 Timed out while waiting for generation state

## Debug Topics

### Debugging incomplete I2C transaction errors (-304321)
1. On a 1487, there are three loopback scripts for odd channels, even channels, or both. Ensure the correct script is used for the desirced serial channel.
2. A camera may not be powered on. Double checking the Power over Coax settings are correct to power on the camera from the PXIe-148x acquisition board.
3. Wire up the script output from Run Configuration Script VI and review for clues on where the problem is occurring
   1. TODO: insert screenshot

### Potential problems
Low frame rate on image display / My images don't show up
Packet timing errors
VI 'hangs' after acquisition completes (display too many logged packets)
I don't see my logged I2C packets?
I don't see my expected GPIO line change
Bitfile build failure (could list as a 'resolution' common things that people try to strip out to free space)
What kind of ECU problems have we seen?
What kind of TAP problems have we seen?

### Techniques
- Adding the 'serial input channel status' or 'serial channel status' (for output) cluster inside the host GSE
- Deep dive on each of the status indicators inside the serial channel status clusters, possible causes, etc.
- Question: Shouldwe have a subvi that exposes the channel status more easily in the host VI?
Adjust frame/line/pixel skip settings to reduce system pressure
Tweaks / adjustments to various 'block sizes' used in FIFOs?






Misc Notes:
TODO: Figure out where to put this: csi2serdesconfig.exe tool how to use it for internal purposes. But not really a debug topic.


## Related Documents
- [PXIe-148X Getting Started Example - Generation Help](./gse-gen-help.md)
- [PXIe-148X Getting Started Example - Acquisition Help](./gse-acq-help.md)
- [PXIe-148X Getting Started Example - Tap Help](./gse-tap-help.md)
