# PXIe-148X Production Test User Guide
{: .no_toc }

This user guide highlights optimizations made to the PXIe-148X module software support to improve the user experience for production test development.

> Note: Software features referenced in this document are included with the NI-FlexRIO 24Q1 driver and later.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Using the Wait for PoC Safe to Disconnect Feature
Disconnecting and connecting coaxial cables to a PXIe-148X module with power-over-coax (PoC) enabled may result in damaged hardware. See [Power Over Coax (PoC)](../reference/hardware/powerovercoax.md) for details.

## Optimizing Configuration Script Execution
Optimization of Serializer/Deserializer (SerDes) configuration script execution differs depending on the type of SerDes component being configured, [FPD-Link](#fpd-link) or [GMSL](#gmsl).

### FPD-Link
The FPD-Link SerDes components use configuration scripts with Python (.py) file format. The Python configuration scripts require using the SerDes Configuration Utility. See [Using the SerDes Configuration Utility](#using-the-serdes-configuration-utility) for details. 

### GMSL
The GMSL SerDes components use configuration scripts with C++ (.cpp) file format. Although GMSL scripts may be executed with the SerDes Configuration Utility, the best optimization is achieved by parsing and executing configuration scripts within LabVIEW. See [Parsing and Running GMSL Configuration Scripts in LabVIEW](#parsing-and-running-gmsl-configuration-scripts-in-labview) for details.

## Using the SerDes Configuration Utility
The SerDes Configuration Utility is a command line utility that is invoked from within a LabVIEW host VI. A description and usage details for the SerDes Configuration Utility is found in the [PXIe-148X Configuration Scripts User Guide](../reference/gettingstartedexample/config-scripts-user-guide.md#serdes-configuration-utility).

### FlexRIO LabVIEW Host Utility VIs
The **Launch SerDes Configuration Utility Server** and **Run Script In SerDes Configuration Utility** LabVIEW VIs are located in the FlexRIO Automotive Vision Utility palette.

![FlexRIO Automotive Vision Utility Palette](../images/FlexRIO-Automotive-Vision-Utility-Palette.png)

![Launch SerDes Configuration Utility Server VI](../images/Launch-SerDes-Configuration-Utility-Server.png)

The **Launch SerDes Configuration Utility Server** VI launches the command line utility, which runs as a TCP socket server, then makes a client connection to the server. The TCP socket connection ID is returned through the **socket connection id out** output. The server remains running in the background until the client connection is terminated, either by calling the TCP Close Connection VI or when the LabVIEW process with the client connection is stopped. See the LabVIEW context help for complete function documentation including optional input parameters **tcp socket config**, **verbose (F)** and **output file**.

![Run Script In SerDes Configuration Utility VI](../images/Run-Script-In-SerDes-Configuration-Utility.png)

The **Run Script In SerDes Configuration Utility** VI sends a client command to the SerDes Configuration Utility server to run the specified **configuration script** on all I<sup>2</sup>C bus instances associated with the serial channels included in the **serial channels** input parameter. See the LabVIEW context help for complete function documentation.

### LabVIEW Block Diagram Example
The block diagram below provides an example of a production test sequence using the SerDes Configuration Utility. Notice that the SerDes Configuration Utility is launched one time and used to run the configuration script on multiple devices under test (DUTs). The SerDes Configuration Utility opens a FlexRIO FPGA session, which introduces significant overhead. By keeping the server open while testing multiple DUTs, the overhead is only incurred once.

![SerDes Configuration Utility Example Block Diagram](../images/SerDes-Configuration-Utility-Example.png)

## Parsing and Running GMSL Configuration Scripts in LabVIEW
GMSL scripts do not contain any conditional logic that requires running in the SerDes Configuration Utility. Therefore, GMSL scripts are parsed as text into a sequence of I<sup>2</sup>C commands and executed in LabVIEW using the FlexRIO I<sup>2</sup>C API functions, which eliminates the dependency and additional overhead of the SerDes Configuration Utility.

### FlexRIO LabVIEW Host Utility VIs
The **Parse SerDes Configuration Script** and **Run SerDes Configuration Commands** LabVIEW VIs are located in the FlexRIO Automotive Vision Utility palette.

![FlexRIO Automotive Vision Utility Palette](../images/FlexRIO-Automotive-Vision-Utility-Palette.png)

![Parse SerDes Configuration Script VI](../images/Parse-SerDes-Configuration-Script.png)

The **Parse SerDes Configuration Script** VI parses a GMSL configuration script (.cpp file format) and returns and array of SerDes configuration commands through the **serdes config commands** output. Commands in GMSL scripts are parsed into I<sup>2</sup>C write, I<sup>2</sup>C transfer or wait command types. See the LabVIEW context help for complete function documentation.

![Run SerDes Configuration Commands VI](../images/Run-SerDes-Configuration-Commands.png)

The **Run SerDes Configuration Commands** VI runs a sequence of SerDes configuration commands defined in the **serdes config commands** input on all I<sup>2</sup>C bus instances associated with the serial channels included in the **serial channels** input parameter. Enabling the **enable debug output (F)** input parameter creates an array of **debug output** strings. The format of the debug output strings matches the verbose output provided by the SerDes Configuration Utility.

### LabVIEW Block Diagram Example
The block diagram below provides an example of a production test sequence using the **Parse SerDes Configuration Script** and **Run SerDes Configuration Commands** VIs. Parsing the configuration script in LabVIEW eliminates the need to launch a separate command line utility which opens and closes a separate FlexRIO FPGA session. Also, the file IO needed to read the contents of the configuration script file and the parsing logic is only performed once. Alternatively, saving the array of SerDes configuration commands as a constant after running the parsing VI one time, eliminates file I/O and parsing on all subsequent runs.

![SerDes Configuration Script Parse Example Block Diagram](../images/SerDes-Configuration-Script-Parse-Example.png)

## Creating a Custom Configuration Script Format
Regardless of the SerDes component type, creating a custom configuration script format is possible with minimal additional effort. The benefits of a custom configuration script file format include:
- Eliminate the dependency and additional overhead of the SerDes Configuration Utility
- Simple, readable configuration script file format
- Able to run on any SerDes type

The **Parse SerDes Configuration Script** VI could be modified or used as a template for adding a custom configuration script file format. Retaining the **serdes config commands** output format allows running the parsed commands with the **Run SerDes Configuration Commands** VI. The **serdes config commands** output is an array of **SerDes Configuration Command** types (shown below). A file format of comma separated values (.csv) with a column for each of elements in the SerDes configuration command cluster would allow for simple parsing into SerDes configuration commands without the added complications from comments and whitespace in the C++ file format.

![SerDes Configuration Command Type](../images/SerDes-Configuration-Command-Type.png)

SerDes Configuration Command Type Elements:
- **command type** - An enumeration that includes wait, write, transfer, read (unsigned 16-bit integer).
- **address** - The register address used for read, write and transfer commands (unsigned 16-bit integer).
- **write data** - An array of bytes to write for write and transfer commands (unsigned 8-bit integers).
- **num bytes to read** - The number of bytes to read for read and transfer commands (unsigned 32-bit integer).
- **milliseconds to wait** - The number of milliseconds to wait for a wait command (unsigned 32-bit integer).

## Related Documents
- [PXIe-148X Configuration Scripts User Guide](../reference/gettingstartedexample/config-scripts-user-guide.md)
- [PXIe-148X Power Over Coax](../reference/hardware/powerovercoax.md)
