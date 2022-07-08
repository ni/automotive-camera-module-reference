# PXIe-148X Configuration Scripts User Guide

Learn how to use the included configuration scripts with the FlexRIO integrated I/O getting started examples for each hardware configuration of your FlexRIO PXIe-148X GMSL or FPD-Link interface module to acquire images or, for serializer-deserializer variants, to tap the data flow between the source and destination for image data.

**Note:** This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be appliable.

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
