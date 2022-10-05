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
2. Update Common Scenarios with Possible Causes and Troubleshooting Options.
3. (Nick) Write section - Host-FPGA General Debugging Workflow
4. (Josh) Create SI and SO Channel Status SubVIs and Add to All 3 GSEs
5. Write sections - Get serial input channel status from the FPGA & Get serial output channel status from the FPGA
6. (Nick) Write section - Instrumenting and Monitoring FPGA Behavior
7. (Josh) Write section - Optimize System Settings for Streaming Performance
8. Look in csi2serdesconfig source for list of unique error codes and add to table (timebox 2 hours)
---

## Error/Warnings Codes

| Error | Explanation | Possible Causes | Troubleshooting Options |
|-|-|-|-|
| -1210 | Multiple entries for the same serial channel detected in serial channel configurations array. | Self explanatory | - Verify the Serial Channel control does not contain duplicates. |
| -1210 | Data type %s is invalid (Serial Channel Display Parameters) | | |
| -1210 | Invalid GPIO bank | The value selected in the **GPIO Bank Control** is not a valid choice for the board.  | - Verify the GPIO value in the control. <br />- Review the Host-FPGA General Debugging Workflow |
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
QUESTION: Is not showing images and low FR different issues?
This may need to merge with some of the FIFO debugging?

### How to debug Generation GSE packet timing errors and FIFO overflows
- Add link to the 'serial channel status' instructions below
 
- Examine DRAM status in SCSI
  - Most useful for output as a sanity check that data is going in and coming out. (quick and dirty check)
  - Explain how DRAM status values can tell you something
  - More useful for output sanity check b/c there's no DMA pushback on Gen. Any pushback is from FPGA if seen.
  - Can be used for rough input/output rate.
  
Open question: Can we distinguish between 'system can't handle it' and 'FPGA can't handle it'?
- Disable display (or shrink image size)
- Adjust frame/line/pixel skip settings to reduce system pressure
  - Link to PXIe-148X Getting Started Example - Common Acquisition Tutorials section
- Use smaller packets (reduce bandwidth requirements)
- Make your timing error limit so high that we don't error out, and then see if DRAM manager still fills up
- Make a known good set of gen data with the timing desired characteristics 
- Reduce channel count.

### How to debug Acquisition FIFO overflow
- Add link to the 'serial input channel status' instructions below

Usually trying to debug an overflow. Can check if DRAM manager overflowing or if the overflow is somewhere else.
You can't easily distinguish if DMA is pushing back on DRAM, causing DRAM overflow (e.g. system can't receive data fast enough, this can be many root causes), or if it's just data coming in faster than we can handle (FPGA simply can't handle this data rate)

- If downstream is not ready for output and you overflow, likely it's downstream at issue (DMA pushing back)
- If downstream is ready, and you overflow, DRAM manager might not be able to handle. This scenario is generally unlikely based on empirical testing

Things we can suggest to determine if it's 'system can't handle it' vs 'FPGA can't handle it' vs ???
- Double check active Lane speeds / count
- Look at SCSI for patterns, are problems following a specific channel always
- Verify DRAM filling, try to compare rate of DRAM filling vs FIFO transfer rate to host
- Instrument the datapath to look for specific hard to detect states? (This may turn into a big doc of its own?)
- Adjust frame/line/pixel skip settings to reduce system pressure
  - Link to PXIe-148X Getting Started Example - Common Acquisition Tutorials section

- TODO: Look up the error for this and decide if it's a separate Tap-specific issue
- Fifo overflow scenario that Daniel explained where reduced gen lane count on a tap board couldn't keep up with a 'burst' on the acq side (even though 'average bandwidth' should have been fine)

### How to debug issues with generation not starting
Host\Gen\API\Configure Serial Output Channels.vi
timestamp buffer start threshold
packet data buffer start threshold (bytes)
dram partition start threshold (bytes)

Take a look at FPGA\Gen\SubVIs\Monitor Ready For Start Condition.vi and document the logic on what triggers start. Some of these values come from host.

## General Debugging Best Practices

### Host-FPGA General Debugging Workflow
- explain how we bubble up FPGA detected errors to the host and how a user can drill down to find the relevant FPGA source.
- Explain the boolean -> case structure architecture

### Get serial input channel status from the FPGA
Add screenshots and workflow showing best way to add the channel status indicator to the Acq GSE.
Add mini indicator reference section that explains each indicator
 - What does the indicator mean (simple explanation)
 - What kinds of scenarios can you sniff out based on the indicator behavior

### Get serial output channel status from the FPGA
Add screenshots and workflow showing best way to add the channel status indicator to the Gen GSE.
Add mini indicator reference section that explains each indicator
 - What does the indicator mean (simple explanation)
 - What kinds of scenarios can you sniff out based on the indicator behavior

### Instrumenting and Monitoring FPGA Behavior
When you say 'monitor some FPGA signals' it means either log every cycle and kick through fifo, or add indicators and view on host (lossy), or you add counters/logic and make a cheap logic analyzer in diagram.

### Optimize System Settings for Streaming Performance
- (BIOS config, etc)
- Look for Chimera notes
- Or source from Neil F's original docs
- RAM, disk, BIOS, limiting memory/interrupt intensive ops

---

## Related Documents
- [PXIe-148X Getting Started Example - Generation Help](./gse-gen-help.md)
- [PXIe-148X Getting Started Example - Acquisition Help](./gse-acq-help.md)
- [PXIe-148X Getting Started Example - Tap Help](./gse-tap-help.md)
