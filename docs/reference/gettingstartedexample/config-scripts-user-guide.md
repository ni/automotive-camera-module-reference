# PXIe-148X Configuration Scripts User Guide
{: .no_toc }

Learn how to use the included configuration scripts with the FlexRIO integrated I/O getting started examples for each hardware configuration of your FlexRIO PXIe-148X GMSL or FPD-Link interface module to acquire images or, for serializer-deserializer variants, to tap the data flow between the source and destination for image data.

**Note:** This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be applicable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Acquisition Scripts

Channels are straightforward to configure with scripts for most PXIe-148X variants because each channel corresponds to a single physical SerDes on the module.

The 8-In variant of the PXIe-1487, however, uses a shared dual deserializer for adjacent channel pairs (e.g. SI0 and SI1, SI2 and SI3, and so on). As a result, the script you need to use for this variant depends on whether you are using the even, odd, or both channels in any given pair.

### Acquisition from an IMX490 camera

Acquisition from an IMX490 camera uses one or more serial input channels to acquire CSI-2 data from one or more LI-IMX490 cameras from Leopard Imaging.

#### Hardware Requirements
1. PXIe-148X Serial Input Channel(s) (Either on a 4 In 4 Out or 8 In Module)
2. Depending on your PXIe-148X model, LI-IMX490-FPDLinkIII or LI-IMX490-GMSL2 camera(s)
3. FAKRA cable(s)

#### Setup
See the acquisition getting started example tutorials.

#### Configuration
Depending on your PXIe-148X model and the channels you are using, choose the right script.

| **Interface Module**               | **Configuration Script**                                                   |
|------------------------------------|----------------------------------------------------------------------------|
| PXIe-1486 (8 In)                   | Host\\Scripts\\DS90UB954\\Acq\\LI\\IMX490_2880x1280_RAW12.py               |
| PXIe-1486 (4 In 4 Out)           | Host\\Scripts\\DS90UB954\\Acq\\LI\\IMX490_2880x1280_RAW12.py               |
| PXIe-1487 (8 In) Even Channels     | Host\\Scripts\\MAX9296A\\Acq\\LI\\IMX490_2880x1280_RAW12_ID1_A.cpp         |
| PXIe-1487 (8 In) Odd Channels      | Host\\Scripts\\MAX9296A\\Acq\\LI\\IMX490_2880x1280_RAW12_ID1_B.cpp         |
| PXIe-1487 (8 In) Adjacent Channels | Host\\Scripts\\MAX9296A\\Acq\\LI\\IMX490_2880x1280_RAW12_ID01_RevSplit.cpp |
| PXIe-1487 (4 In 4 Out)           | Host\\Scripts\\MAX9296A\\Acq\\LI\\IMX490_2880x1280_RAW12_ID1_B.cpp         |

### Acquisition from DS90UB953 Pattern Generator (PXIe-1486 Only)

If you do not have a camera to use for Acquisition, you may use any DS90UB953 serializer (such as from an eval board or PXIe-1486 Serial Output Channel) in pattern generation mode. A Serial Input Channel may then be used to acquire CSI-2 data from the serializer.

#### Hardware Requirements
1. PXIe-1486 Serial Input Channel(s) (Either on a 4 In 4 Out or 8 In Module)
2. DS90UB953 Serializer(s)
3. FAKRA cable(s)

#### Setup
See the acquisition getting started example tutorials.

#### FPD-Link III Configuration
Configuration Script: `Host\Scripts\DS90UB954\Acq\953_1920x1080_RAW12_PatGen.py`

## Tap Scripts

You can use the Acquisition example in conjunction with the Tap example to demonstrate the tap capability of the PXIe-148X 4 In/4 Out modules. These modules can "tap" the line between a serial output (the camera) and the PXIe-148X module used for acquisition. First run the Tap example to configure the PXIe-148X 4 In/4 Out module to act as a tap. Once the tap module is configured and running, run the Acquisition example as described earlier, with no need to modify the acquisition scripts.

### FPD-Link III Tap Setup
Deserializer (Input) Configuration Script: `Host\Scripts\DS90UB954\Tap\Des_Tap.py`

Serializer (Output) Configuration Script:  `Host\Scripts\DS90UB953\Tap\Ser_Tap.py`

### GMSL2 Tap Setup
In a tap configuration, the GMSL2 Serial Output Channel must be aware of the configuration of the deserializer it is linked with. If multiple tap serializers are sending data to a single destination deserializer, then each serializer must be configured to have a different packet ID. Additionally, the user must ensure that the Stream ID and Data Type of the tap configuration matches the expected Stream ID and Data Type of the link being tapped. The included scripts are all compatible with the above acquisition examples.

Deserializer (Input) Configuration Script: `Host\Scripts\MAX9296A\Tap\RAW12_ID01_Des_Tap.cpp`

Serializer (Output) Configuration Script: `Host\Scripts\MAX9295A\Tap\RAW12_ID1_Ser_Tap.cpp`

In the special case mentioned above where two or more tap serializers are linked to the same deserializer, the following script should be used alongside the above serializer script to drive both inputs of the deserializer:
`Host\Scripts\MAX9295A\Tap\RAW12_ID0_ToRevSplit_Ser_Tap.cpp`

## SerDes Configuration Utility
The SerDes configuration utility is used by the PXIe-148X Getting Started Example to run configuration scripts for serializers and deserializers by executing a sequence commands over the I<sup>2</sup>C bus. The GMSL scripts are C++ files that are formatted as comma separated hexadecimal byte values, which are parsed into a sequence of I<sup>2</sup>C write or transfer commands and executed by the utility. The FPD-Link configuration scripts are Python files with runtime logic such as variables and if/else statements that prevent parsing into I<sup>2</sup>C commands. The SerDes configuration utility is written in Python to allow execution of FPD-Link configuration scripts in the Python interpreter. For Windows distributions the SerDes configuration utility is compiled into an executable and installed in `C:\Users\Public\Documents\National Instruments\FlexRIO`. For Linux distributions the utility is installed in `/usr/share/ni-flexrio/csi2serdesconfig`.

FlexRIO driver version 24Q1 and later uses the SerDes configuration utility [serdes_config_util_server](#serdesconfigutilserver).

FlexRIO driver version 23Q4 and earlier uses the SerDes configuration utility [niflexriocsi2serdesconfig](#niflexriocsi2serdesconfig).

### serdes_config_util_server
The serdes_config_util_server utility runs as a TCP socket server and the LabVIEW host code acts as TCP socket client. In LabVIEW, the utility is launched using the <font face="courier new">System Exec.vi</font> with the wait until completion parameter disabled. Configuration scripts are run by sending TCP write commands to the utility. The server is closed when the client closes the TCP connection. Operating as a server allows the utility to open a single FlexRIO API session to run multiple scripts, which improves the script execution time when running more than one script compared to the niflexriocsi2serdesconfig utility. To update applications created with GSE API VIs prior to FlexRIO driver version 24Q1, install FlexRIO driver version 24Q1 or later and replace <font face="courier new">Run Configuration Script.vi</font> in the application with the new GSE API VI located in `Host\Common\API`. Usage information for the utility is shown below.

```
usage: serdes_config_util_server.exe [-h] [--host HOST] [--port PORT] [--timeout TIMEOUT] [-s [{1486,1487}]] [-v] [-o OUTPUT] resource bitfile

NI-FlexRIO Command Line Socket Server Utility for configuration of PXIe-148X CSI-2 serializers, deserializers, and camera modules.

positional arguments:
  resource              the name of the RIO resource to use for configuration
  bitfile               the path of the bitfile to use for configuration

optional arguments:
  -h, --help            show this help message and exit
  --host HOST           the socket hostname in internet domain notation or an IPv4 address
  --port PORT           the socket port number
  --timeout TIMEOUT     the socket timeout in seconds to wait for a client connection
  -s [{1486,1487}], --simulated_module [{1486,1487}]
                        enable execution of utility without opening a FlexRIO session for testing script syntax
  -v, --verbose         increase output verbosity
  -o OUTPUT, --output OUTPUT
                        redirect stdout to the specified output file
```

### niflexriocsi2serdesconfig
The niflexriocsi2serdesconfig runs a single configuration script for each execution of the utility. It opens a FlexRIO API session, runs a configuration script and closes the FlexRIO API session each time it is run. In LabVIEW, the utility is launched using the <font face="courier new">System Exec.vi</font> with the wait until completion parameter enabled. Usage information for the utility is shown below.

```
usage: niflexriocsi2serdesconfig.exe [-h] [-s [{1486,1487}]] [-v] [-w]
                                     [-p POSC]
                                     resource bitfile serdes script

NI-FlexRIO Command Line Utility for PXIe-1486 and PXIe-1487 CSI-2 serializers, deserializers, and camera modules.

positional arguments:
  resource              the name of the RIO resource to use for configuration
  bitfile               the path of the bitfile to use for configuration
  serdes                the name of the i2c bus instance to use for configuration
  script                the configuration script to execute

optional arguments:
  -h, --help            show this help message and exit
  -s [{1486,1487}], --simulated_module [{1486,1487}]
                        enable execution of utility without opening a FlexRIO session for testing script syntax
  -v, --verbose         increase output verbosity
  -w, --wait            wait for keypress
  -p POSC, --posc POSC  create posc config binary file
```