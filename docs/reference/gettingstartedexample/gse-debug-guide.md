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
The overall architecture of our Host and FPGA example code is to have the host query and poll at appropriate times to pass the status of the FPGA to the user. Errors that are detected at runtime on the FPGA are bubbled up through boolean status signals on the FPGA front panel. The host will read these signals and set an error message so that the source of the error can be found. Errors that are set will tell the user where to start looking for errors. Looking at the host code will tell you which portions of the FPGA design to look at. The signals are latched until the host api calls reset or FPGA diagram reset. 
TODO - Image (Example GPIO write, checks a bool and sets a status)

Aditionally the host will check certain status's that can give you clues about how or why an error occured. The Acquisition and Generation states are queried and will display "Error" if something has gone wrong.
TODO Image (Get Acq State image example)
TODO - Image (Check channel status generation image example)

There are other times where we wait for a certain state to be returned from the FPGA. If a host side timeout occurs while waiting for that state, we will set an error.
TODO - Image (Wait for Acquisition state error)

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
When debugging or investigating FPGA behavior, there are some general strategies that you can implement to figure out where errors are coming from. You can monitor the boolean signals that we pass to the user through the FPGA front panel, or you can monitor various internal state machines. The exact nature of what you should instrument is dependent on the problem you are trying to solve, but there are three common strategies for getting debug information out of the LabVIEW FPGA image at runtime.

#### Add additional indicators that allow you to poll and view the information from the host. 
Adding indicators will alloww you to read the status back of intersting signals from the host. This will give you a "last updated" view of the signals in question. It will not allow you to monitor every change to a signal but can give you good insights.
TODO - image

#### You can latch signals and add new indicators
Dropping down a basic latch when a certain condition occurs can allow you to save a particular state. Make sure to wire up the reset signal so that you can clear your state. This will allow you to capture a specific change on the FPGA.
TODO - image

#### You can count rising and falling edges to see where things are going wrong
This is a good way to measure boolean signals to see how many times a certain condition occurs. This can give you insights into complexe data flow problems wondering how many times a gate or state is allowing valid data to flow.
TODO - image

#### Add an FPGA to Host FIFO to send up debug information sideband.
This technique takes a long time to setup, can dramatically change the resource utilization, and can change overall system bandwidth usage. It is the best way to instrument every single change on a cerain signal and pipe it up to the host. Having the host pull data from the fifo and store it can lead to bandwidth and file system concerns, so only use this technique if you really need to capture every change on a signal in the FPGA.
TODO - image

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
