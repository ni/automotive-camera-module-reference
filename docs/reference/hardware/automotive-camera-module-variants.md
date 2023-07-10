# PXIe-148X Automotive Camera Module Details
{: .no_toc }

This document contains reference information on various properties of PXIe-148X automotive camera modules and links to their related specifications.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Table Heading Definitions

| Heading | Definition |
|-|-|
| Model Number             | Generic model name associated with a family of products. Often used in shipping example code and help documentation. |
| Channels                 | Number and type of serial channels. |
| Protocol                 | Supported CSI-2 protocol and generation. |
| Serializer               | Specific serializer of the module, if applicable. |
| Deserializer             | Specific deserializer of the module, if applicable. |
| Slot Count               | The number of physical slots required in a PXIe chassis for a given module. |
| Front Panel Overlay      | Name printed on the front panel of the physical module. |
| NI MAX Name              | The name of a module shown in NI MAX. |
| LabVIEW FPGA Target Name | The name of an FPGA Target for a module shown in a LabVIEW Project. |
| FlexRIO First Supported  | The first NI-FlexRIO driver version to support a given module. |
| Product ID               | FlexRIO driver product ID. Occasionally referenced in example code or help text. |
| Model Alias              | Conditional disable symbol MODEL_ALIAS used in FPGA VIs. Can be shared between similar models. |

---

## PXIe-1486 FlexRIO FPD-Link III Modules

### Hardware Details

| Model Number           | Channels     | Protocol     | Serializer | Deserializer | Slot Count | Front Panel Overlay                    |
|------------------------|--------------|--------------|------------|--------------|------------|----------------------------------------|
| PXIe-1486 Deserializer | 8 input      | FPD-Link III | N/A        | DS90UB954    | 2          | FlexRIO FPD-LINK™ III 954 Deserializer |
| PXIe-1486 Serializer   | 8 output     | FPD-Link III | DS90UB953  | N/A          | 2          | FlexRIO FPD-LINK™ III 953 Serializer   |
| PXIe-1486 SerDes       | 4 in / 4 out | FPD-Link III | DS90UB953  | DS90UB954    | 2          | FlexRIO FPD-LINK™ III 953/954 SerDes   |

#### Block Diagram
![1486 block diagram](../../images/PXIe-1486-block-dia.png)

#### Features & Limitations
- GPIO and I2C voltage is 1.8V on the Serializer and Deserializer. Do not configure the I2C voltage register to 3.3V.
- I2C busses are point-to-point for each SER / DES and the CPLD. They are not shared.
- Default config from boot-strapping voltage dividers is: 
    - DS90UB954: I2C address 0x60 (8b), synchronous mode
    - DS90UB953: I2C address 0x30 (8b), synchronous mode
- Changing the REFCLK on the PLL will reset both components.

### Software Details

| Model Number           | NI MAX Name                                        | LabVIEW FPGA Target Name          | FlexRIO First Supported | Product ID | Model Alias |
|------------------------|----------------------------------------------------|-----------------------------------|-------------------------|------------|-------------|
| PXIe-1486 Deserializer | NI PXIe-1486 (8 In - 954 Deserializer - KU11P)     | NI PXIe-1486 (8 In - KU11P)       | [20.6](#compat-note)    | 0x7A82     | 1486_8I     |
| PXIe-1486 Serializer   | NI PXIe-1486 (8 Out - 953 Serializer - KU11P)      | NI PXIe-1486 (8 Out - KU11P)      | [20.7](#compat-note)    | 0x7A83     | 1486_8O     |
| PXIe-1486 SerDes       | NI PXIe-1486 (4 In 4 Out - 953/954 SerDes - KU11P) | NI PXIe-1486 (4 In 4 Out - KU11P) | [20.6](#compat-note)    | 0x7A84     | 1486_4I_4O  |

---

## PXIe-1487 FlexRIO GMSL2 Modules

### Hardware Details

| Model Number           | Channels     | Protocol | Serializer | Deserializer | Slot Count | Front Panel Overlay                |
|------------------------|--------------|----------|------------|--------------|------------|------------------------------------|
| PXIe-1487 Deserializer | 8 input      | GMSL1/2  | N/A        | MAX9296A     | 2          | FlexRIO GMSL2 9296A Deserializer   |
| PXIe-1487 Serializer   | 8 output     | GMSL1/2  | MAX9295A   | N/A          | 2          | FlexRIO GMSL2 9295A Serializer     |
| PXIe-1487 SerDes       | 4 in / 4 out | GMSL1/2  | MAX9295A   | MAX9296A     | 2          | FlexRIO GMSL2 9295A/9296A SerDes   |
| PXIe-1487 Deserializer | 8 input      | GMSL1/2  | N/A        | MAX96716A    | 2          | FlexRIO GMSL2 96716A Deserializer  |
| PXIe-1487 Serializer   | 8 output     | GMSL1/2  | MAX96717   | N/A          | 2          | FlexRIO GMSL2 96717 Serializer     |
| PXIe-1487 SerDes       | 4 in / 4 out | GMSL1/2  | MAX96717   | MAX96716A    | 2          | FlexRIO GMSL2 96717/96716A SerDes  |
| PXIe-1487 Serializer   | 8 output     | GMSL1/2  | MAX96717F  | N/A          | 2          | FlexRIO GMSL2 96717F Serializer    |
| PXIe-1487 SerDes       | 4 in / 4 out | GMSL1/2  | MAX96717F  | MAX96716A    | 2          | FlexRIO GMSL2 96717F/96716A SerDes |

### Software Details

| Model Number           | NI MAX Name                                              | LabVIEW FPGA Target Name          | FlexRIO First Supported    | Product ID | Model Alias |
|------------------------|----------------------------------------------------------|-----------------------------------|----------------------------|------------|-------------|
| PXIe-1487 Deserializer | NI PXIe-1487 (8 In - 9296A Deserializer - KU11P)         | NI PXIe-1487 (8 In - KU11P)       | [20.6](#compat-note)       | 0x7A85     | 1487_8I     |
| PXIe-1487 Serializer   | NI PXIe-1487 (8 Out - 9295A Serializer - KU11P)          | NI PXIe-1487 (8 Out - KU11P)      | [20.7](#compat-note)       | 0x7A86     | 1487_8O     |
| PXIe-1487 SerDes       | NI PXIe-1487 (4 In/4 Out - 9295A/9296A SerDes - KU11P)   | NI PXIe-1487 (4 In 4 Out - KU11P) | [20.6](#compat-note)       | 0x7A87     | 1487_4I_4O  |
| PXIe-1487 Deserializer | NI PXIe-1487 (8 In - 96716A Deserializer - KU11P)        | NI PXIe-1487 (8 In - KU11P)       | [2022 Q4](#compat-note)    | 0x7A85     | 1487_8I     |
| PXIe-1487 Serializer   | NI PXIe-1487 (8 Out - 96717 Serializer - KU11P)          | NI PXIe-1487 (8 Out - KU11P)      | [2022 Q4](#compat-note)    | 0x7A86     | 1487_8O     |
| PXIe-1487 SerDes       | NI PXIe-1487 (4 In/4 Out - 96717/96716A SerDes - KU11P)  | NI PXIe-1487 (4 In 4 Out - KU11P) | [2022 Q4](#compat-note)    | 0x7A87     | 1487_4I_4O  |
| PXIe-1487 Serializer   | NI PXIe-1487 (8 Out - 96717F Serializer - KU11P)         | NI PXIe-1487 (8 Out - KU11P)      | [2023 Q1](#compat-note)    | 0x7A86     | 1487_8O     |
| PXIe-1487 SerDes       | NI PXIe-1487 (4 In/4 Out - 96717F/96716A SerDes - KU11P) | NI PXIe-1487 (4 In 4 Out - KU11P) | [2023 Q1](#compat-note)    | 0x7A87     | 1487_4I_4O  |

---

## PXIe-1488 FlexRIO FPD-Link IV Modules

### Hardware Details

| Model Number           | Channels     | Protocol    | Serializer | Deserializer | Slot Count | Front Panel Overlay                     |
|------------------------|--------------|-------------|------------|--------------|------------|-----------------------------------------|
| PXIe-1488 Deserializer | 8 input      | FPD-Link IV | N/A        | DS90UB902    | 2          | FlexRIO FPD-LINK™ III 9702 Deserializer |
| PXIe-1488 Serializer   | 8 output     | FPD-Link IV | DS90UB971  | N/A          | 2          | FlexRIO FPD-LINK™ III 971 Serializer    |
| PXIe-1488 SerDes       | 4 in / 4 out | FPD-Link IV | DS90UB971  | DS90UB9702   | 2          | FlexRIO FPD-LINK™ III 971/9702 SerDes   |

### Software Details

| Model Number           | NI MAX Name                                         | LabVIEW FPGA Target Name          | FlexRIO First Supported    | Product ID | Model Alias |
|------------------------|-----------------------------------------------------|-----------------------------------|----------------------------|------------|-------------|
| PXIe-1488 Deserializer | NI PXIe-1488 (8 In - 9702 Deserializer - KU11P)     | NI PXIe-1488 (8 In - KU11P)       | [2023 Q2](#compat-note)    | 0x7AE4     | 1488_8I     |
| PXIe-1488 Serializer   | NI PXIe-1488 (8 Out - 971 Serializer - KU11P)       | NI PXIe-1488 (8 Out - KU11P)      | [2023 Q2](#compat-note)    | 0x7AE5     | 1488_8O     |
| PXIe-1488 SerDes       | NI PXIe-1488 (4 In 4 Out - 971/9702 SerDes - KU11P) | NI PXIe-1488 (4 In 4 Out - KU11P) | [2023 Q2](#compat-note)    | 0x7AE6     | 1488_4I_4O  |

---

## PXIe-1489 FlexRIO GMSL3 Modules

### Hardware Details

| Model Number           | Channels     | Protocol | Serializer | Deserializer | Slot Count | Front Panel Overlay                |
|------------------------|--------------|----------|------------|--------------|------------|------------------------------------|
| PXIe-1489 Deserializer | 4 input      | GMSL3    | N/A        | MAX96792A    | 1          | FlexRIO GMSL3 96792A Deserializer  |
| PXIe-1489 Serializer   | 4 output     | GMSL3    | MAX96793   | N/A          | 1          | FlexRIO GMSL3 96793 Serializer     |
| PXIe-1489 SerDes       | 2 in / 2 out | GMSL3    | MAX96793   | MAX96792A    | 1          | FlexRIO GMSL3 96793/96792A SerDes  |

### Software Details

| Model Number           | NI MAX Name                                              | LabVIEW FPGA Target Name          | FlexRIO First Supported    | Product ID | Model Alias |
|------------------------|----------------------------------------------------------|-----------------------------------|----------------------------|------------|-------------|
| PXIe-1489 Deserializer | NI PXIe-1489 (4 In - 96792A Deserializer - KU11P)        | NI PXIe-1489 (4 In - KU11P)       | [2023 Q3](#compat-note)    | 0x7AE7     | 1489_4I     |
| PXIe-1489 Serializer   | NI PXIe-1489 (4 Out - 96793 Serializer - KU11P)          | NI PXIe-1489 (4 Out - KU11P)      | [2023 Q3](#compat-note)    | 0x7AE8     | 1489_4O     |
| PXIe-1489 SerDes       | NI PXIe-1489 (2 In 2 Out - 96793/96792A SerDes - KU11P)  | NI PXIe-1489 (2 In 2 Out - KU11P) | [2023 Q3](#compat-note)    | 0x7AE9     | 1489_2I_2O  |

---

<a id="compat-note"></a>
> (\*) Full details on hardware and OS compatibility can be found [here](https://www.ni.com/en-us/support/documentation/compatibility/21/ni-hardware-and-operating-system-compatibility.html) on ni.com.

---

## Related Documents

- [PXIe-1486 Getting Started Guide](https://www.ni.com/docs/en-US/bundle/pxie-1486-getting-started/page/intro.html)
- [PXIe-1486 Specifications](https://www.ni.com/docs/en-US/bundle/pxie-1486-specs/page/specs.html)
- [PXIe-1487 Getting Started Guide](https://www.ni.com/docs/en-US/bundle/pxie-1487-getting-started/page/intro.html)
- [PXIe-1487 Specifications](https://www.ni.com/docs/en-US/bundle/pxie-1487-specs/page/specs.html)    
- [PXIe-1488 Getting Started Guide](https://www.ni.com/docs/en-US/bundle/pxie-1488-getting-started/page/intro.html)
- [PXIe-1488 Specifications](https://www.ni.com/docs/en-US/bundle/pxie-1488-specs/page/specs.html)    
- [PXIe-1489 Getting Started Guide](https://www.ni.com/docs/en-US/bundle/pxie-1489-getting-started/page/intro.html)
- [PXIe-1489 Specifications](https://www.ni.com/docs/en-US/bundle/pxie-1489-specs/page/specs.html)    
