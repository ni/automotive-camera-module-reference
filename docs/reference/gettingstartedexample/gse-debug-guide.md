# PXIe-148X Getting Started Example - Troubleshooting Guide
{: .no_toc }

Refer to this document to get insights and suggestions on troubleshooting performance issues, error codes, and problems with using the PXIe-148x Getting Started Examples (GSE).

> Note: This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be appliable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## TODO List
1. Update Error/Warnings Code with Possible Causes and Troubleshooting Options.
1. Update Common Scenarios with Possible Causes and Troubleshooting Options.
---

## Error/Warnings Codes

| Error | Explanation | Possible Causes | Troubleshooting Options |
|-|-|-|-|
| -1210 | Multiple entries for the same serial channel detected in serial channel configurations array. | Self explanatory | - Verify the Serial Channel control does not contain duplicates. |
| -1210 | Data type %s is invalid (Serial Channel Display Parameters) | | |
| -1210 | Invalid GPIO bank | The value selected in the **GPIO Bank Control** is not a valid choice for the board.  | - Verify the GPIO value in the control. <br />- Review the Host to FPGA general debugging strategy |
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

| GSE Type | Scenario | Possible Causes | Troubleshooting Options |
|-|-|-|-|
| Gen | Packet timing errors detected on generation | TODO | TODO |
| Gen | Generation doesn't start after clicking **Serializer Setup Complete** | - You send data with TS too far in the future, DRAM fills but waiting for timer to hit first TS, you might hit system timeout (defaults to 20/30sec TBD?) <br /> - You don't have consistent data stream, can 'go idle'. You pump no data for long enough, it will automatically stop your generation (everything goes to false)| |
| Acq | VI 'hangs' after acquisition completes but does (eventually) stop | Display too many logged packets TODO | |
| Any | I2C packets are not logged as expected | | |
| Any | GPIO line change not logged as expected | | |
| Any | Display **Source Rate (fps)** and **Update Rate (fps)** both lower than expected | System bandwidth cannot support this sustained rate. DMA Bandwidth problem | May be impacted by Skip settings as well because that reduces DMA usage |
| Any | Display **Update Rate (fps)** lower than the display **Source Rate (fps)** | TBD. Source is supposed to be based on how many metadata 'chunks' are being pulled from the FIFO. If you can't keep up pushing to display at same rate you're pulling in source data, Display will be lower (skipping images). | Use Skip Line/Pixels to reduce size and remove need to resample on host. Saves bandwidth and CPU resources. Or use Skip Frames to reduce frames sent if can't be displayed anyway (e.g. HW limitations) |
| Any | Bitfile build failure occurs | Limited resources available in FPGA | | General guidelines on easy pieces to remove/tweak in GSE FPGA VIs |

TODO: Look in csi2serdesconfig source for list of unique error codes
TODO: Consider adding a 'general debugging strategy' that explains how we bubble up FPGA detected errors to the host and how a user can drill down to find the relevant FPGA source.

## Troubleshooting Guides

### How to debug incomplete I2C transaction errors (-304321)
Some comments on general troubleshooting tips to quickly check first
1. On a 1487, there are three loopback scripts for odd channels, even channels, or both. Ensure the correct script is used for the desired serial channel.
2. A camera may not be powered on. Double checking the Power over Coax settings are correct to power on the camera from the PXIe-148x acquisition board.
3. Script may have invalid commands - need to compare camera data sheet vs script commands
4. Others?

Some explanation on how to do a deeper dive
Wire up the script output from Run Configuration Script VI and review for clues on where the problem is occurring
   1. TODO: insert screenshot

### How to debug image display that does not show images or has a low frame rate
This may need to merge with some of the FIFO debugging?

### How to debug packet timing errors


### How do I get more channel status information from the FPGA? (acq)
- Adding the 'serial input channel status' or 'serial channel status' (for output) cluster inside the host GSE

Most useful for output as a sanity check that data is going in and coming out. (quick and dirty check)
dram status
bytes written
bytes read

Usually trying to debug an overflow. Can check if DRAM manager overflowing or if the overflow is somewhere else.
You can't easily distinguish if DMA is pushing back on DRAM, causing DRAM overflow (e.g. system can't receive data fast enough, this can be many root causes), or if it's just data coming in faster than we can handle (FPGA simply can't handle this data rate)

Open question: Can we distinguish between 'system can't handle it' and 'FPGA can't handle it'?
For generation:
- Make your timing error limit so high that we don't error out, and then see if DRAM manager still fills up

For acq:
- TBD?  TODO: How to help someone determine between these two scenarios
- If downstream is not ready for output and you overflow, likely it's downstream at issue (DMA pushing back)
- If downstream is ready, and you overflow, DRAM manager might not be able to handle. This scenario is generally unlikely based on empirical testing

### How do I get more channel status information from the FPGA? (gen)
Most useful for output as a sanity check that data is going in and coming out. (quick and dirty check)
dram status
bytes written
bytes read

More useful for output sanity check b/c there's no DMA pushback on Gen. Any pushback is from FPGA if seen.
Can be used for rough input/output rate.

When you say 'monitor some FPGA signals' it means either log every cycle and kick through fifo, or add indicators and view on host (lossy), or you add counters/logic and make a cheap logic analyzer in diagram.


elements per DRAM bank

Host\Gen\API\Configure Serial Output Channels.vi
timestamp buffer start threshold
packet data buffer start threshold (bytes)
dram partition start threshold (bytes)

Take a look at FPGA\Gen\SubVIs\Monitor Ready For Start Condition.vi and document the logic on what triggers start. Some of these values come from host.


#### TODO: Optimal system settings for streaming (BIOS config, etc)
- Look for Chimera notes
- Or source from Neil F's original docs
- RAM, disk, BIOS, limiting memory/interrupt intensive ops
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
-  - probably we meant 'element size in fifos. we will not document, it's all empirically determined magic numbers that are optimal already'.
- TODO: Figure out where to put this: csi2serdesconfig.exe tool how to use it for internal purposes. - But not really a debug topic.
- Should we show people how to make host side changes to enable >2 displays?
- Frame sync usage techniques? How/when to use it.
- Tips and tricks for simulating continuous generation with a gen board
---

Discussion with Jared




## Related Documents
- [PXIe-148X Getting Started Example - Generation Help](./gse-gen-help.md)
- [PXIe-148X Getting Started Example - Acquisition Help](./gse-acq-help.md)
- [PXIe-148X Getting Started Example - Tap Help](./gse-tap-help.md)
