# PXIe-148X Getting Started Example - Common Generation Tutorials
{: .no_toc }

** IN WORK - DOCUMENTATION IS INCOMPLETE AND IN ACTIVE DEVELOPMENT **

This document covers a range of common scenarios using the PXIe-148X Generation Getting Started Example (GSE) to help you understand LLP generation, I2C and GPIO timestamping, and common configuration options.

> Note: This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be appliable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Prerequisites

## Generation and Acquisition Topics

### Performing a Simple Generation and Acquisition

### Generating and Displaying I2C Timestamps

## Generating and Filtering LLP Packets

## Playing Back Previously Acquired Data

## Setting FPGA Display Parameters

This tutorial shows you how to configure the **FPGA Display Parameters** to change the images displayed during generation.

1. Set the following controls on the Generation Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |
    | Resource | TDMS File Directory| A directory path containing a file named <font face = "courier new">SI0_LLP_Packets.tdms</font> |
    
    > Note: The **TDMS File Directory** is a folder selection and the browse dialog shows folders only, not file names. 
    >
    > Use the default **TDMS File Directory** value if generating from a [TDMS file created with the Create CSI-2 Packet TDMS Files utility's default settings](gse-gen-basic.md#create-tdms-files-for-generation).

### Displaying Generated Images

1. Follow the procedure defined in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#displaying-acquired-images) with the following modifications.
    - Replace all references to <font face = "courier new">SI0</font> with <font face = "courier new">SO0</font>.
    - When running the Generation Example VI, wait for the **Waiting for Serializer Setup** indicator to illuminate then click **Serializer Setup Complete**.

    > The logic for FPGA display parameters is shared for all PXIe-148X getting started examples.

### Reducing System Bandwidth Usage

1. Follow the procedure defined in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#reducing-system-bandwidth-usage) with the following modifications.
    - When running the Generation Example VI, wait for the **Waiting for Serializer Setup** indicator to illuminate then click **Serializer Setup Complete**.

    > The logic for FPGA display parameters is shared for all PXIe-148X getting started examples.

## Setting RAW Display Parameters

This tutorial shows you how to configure the **RAW Display Parameters** to change the interpretation of images being displayed during generation.

1. Set the following controls on the Generation Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |
    | Resource | TDMS File Directory| A directory path containing a file named <font face = "courier new">SI0_LLP_Packets.tdms</font> |

    > Note: The **TDMS File Directory** is a folder selection and the browse dialog shows folders only, not file names. 
    >
    > Use the default **TDMS File Directory** value if generating from a [TDMS file created with the Create CSI-2 Packet TDMS Files utility's default settings](gse-gen-basic.md#create-tdms-files-for-generation).

2. Follow the procedure defined in [PXIe-148X Getting Started Example - Common Acquisition Tutorials](gse-acq-common.md#setting-raw-display-parameters) with the following modifications.
    - Replace all references to <font face = "courier new">SI0</font> with <font face = "courier new">SO0</font>.
    - When running the Generation Example VI, wait for the **Waiting for Serializer Setup** indicator to illuminate then click **Serializer Setup Complete**.

    > Note: The logic for setting RAW display parameters is shared for all PXIe-148X getting started examples, but results will not match the procedure descriptions exactly if using a TDMS file created with the Create CSI-2 Packet TDMS Files utility.

## Setting Serial Channel Configurations

This tutorial shows you how to configure the **Serial Channel** tab to generate and display images on multiple serial output channels.

### Creating TDMS Files to Generate

1. Open the Create CSI-2 Packet TDMS Files utility in the LabVIEW project.

2. Run the VI with all default settings to generate a TDMS file for channel SI0 containing 10 frames at 1920x1080 resolution.

3. Make the following modifications on the Create CSI-2 Packet TDMS Files utility front panel.
    - Set the value at index zero of the **Serial Input Channels** array to <font face = "courier new">SI1</font>.
    - Make the following modifications to the **Frame Data Configuration** control.
        - Set the **horizontal resolution** control value to <font face = "courier new">2880</font>.
        - Set the **vertical resolution** control value to <font face = "courier new">1280</font>.

4.  Run the VI to generate a TDMS file for channel SI1 containing 10 frames at 2880x1280 resolution.

### Generating and Displaying Multiple Images

1. Set the following controls on the Generation Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Control | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |

2. Select the **Serial Channel** tab and make the following modifications.
    - Select index <font face = "courier new">0</font> of the **Channel Configurations** array and make the following modifications.
        - Select **Grayscale** on the **Interpretation** control in the **RAW Display Parameters** cluster.
    - Select index <font face = "courier new">1</font> of the **Channel Configurations** array and make the following modifications.
        - Set the **Serial Channel** control value to <font face = "courier new">SO1</font>.
        - Select **Grayscale** on the **Interpretation** control in the **RAW Display Parameters** cluster.

3. Run the VI, wait for the **Waiting for Serializer Setup** indicator to illuminate, and click the **Serializer Setup Complete** control button to start the generation.
    > Once the generation completes, notice the **Number of Generated Packets** indicator array shows a value of <font face = "courier new">10820</font> at index zero and <font face = "courier new">12820</font> at index one, which equals the number of packets per frame (vertical resolution) times the number of frames generated for each channel. The indices in the **Number of Generated Packets** array correspond to the indices in the **Channel Configurations** array.

4. Click the **First Display Channel** tab to view the last frame displayed for SO0.
    > Note that the image resolution matches the 1920x1080 resolution of the TDMS file created for SI0.

5. Click the **Second Display Channel** tab to view the last frame displayed for SO1.
    > Note that the image resolution matches the 2880x1280 resolution of the TDMS file created for SI1.

6. Select the **Serial Channel** tab and set the **Start Trigger Delay (s)** control value to <font face = "courier new">2</font>.

7. Run the VI, wait for the **Waiting for Serializer Setup** indicator to illuminate, and click the **Serializer Setup Complete** control button to start the generation. Notice that there is now a 2 second delay before images are generated after clicking the **Serializer Setup Complete** button.

## Using the General Purpose Input/Output (GPIO) Lines

This tutorial shows you how to configure routing of GPIO lines between GPIO banks as well as manually reading and writing to the GPIO banks on the PXIe-148X interface module. This tutorial modifies GPIO line values while the generation is running, logs the GPIO timestamps to disk and displays digital waveforms of the GPIO lines.

> Note: Timestamps are relative to a time immediately after the FPGA bitfile is downloaded and run, not the start of the generation. This allows capturing I2C and GPIO timestamps during configuration before the generation starts.

1. Set the following controls on the Acquisition Example GSE VI and leave all other values at their defaults.
    > Note: VI controls and indicators can be reset to default values by clicking on the **Edit** menu and selecting the **Reinitialize Values to Default** option.

    | Tab | Setting | Value |
    |---|---|---|
    | Resource | RIO Device | [System Specific] |
    | Resource | Bitfile Path | [Refer to Bitfile Path in the PXIe-148X Generation GSE Help](../../reference/gettingstartedexample/gse-gen-help.md#table-of-pxie-148x-generation-bitfiles) |
    | Resource | Display Generated Images | Disabled |
    | Resource | Log GPIO to Disk | Enabled |

### Creating a TDMS File for the GPIO Tutorial

1. Open the Create CSI-2 Packet TDMS Files utility in the LabVIEW project.

2. Make the following modifications on the Create CSI-2 Packet TDMS Files utility front panel.
    - Set the **Number of Frames** control to <font face = "courier new">60</font>
    - Update the **Frame Data Configuration** cluster with the following settings.
        - Set the **horizontal resolution** control value to <font face = "courier new">640</font>.
        - Set the **vertical resolution** control value to <font face = "courier new">480</font>.
    - In the **Frame Timing Configuration** cluster, set the **desired frame rate (fps)** control to <font face = "courier new">1</font>.

3. Run the VI to generate a TDMS file for channel SI0 containing 60 frames at 640x480 resolution.
    > The frame rate of 1 frame per second will cause the generation to run for 1 minute, which will allow enough time to manually read and write to GPIO during the generation.

### Manually Reading and Writing to GPIO

This tutorial section shows you how to perform manual reads and writes to GPIO during generation and display digital waveforms for the resulting GPIO timestamp data.
> Note: The [Creating a TDMS File for the GPIO Tutorial](#creating-a-tdms-file-for-the-gpio-tutorial) section should be run prior to running the steps in this tutorial.

1. Select the **GPIO** tab and make the following modifications.
    - Update the **GPIO to Display** array with the following settings.
        - At index 0 select **Ser0 GPIO** in the **GPIO Bank** control and set the **GPIO Number** to <font face = "courier new">0</font>. 
        - At index 1 select **Ser0 GPIO** in the **GPIO Bank** control and set the **GPIO Number** to <font face = "courier new">3</font>.

        > Setting these values enables display of GPIO traffic for GPIO line 0 and line 3 on the SO0 channel.
        >
        > The **GPIO to Display** control specifies GPIO lines to display on the GPIO Timestamps Waveform after the acquisition completes. Timestamps for GPIO lines not included in the **GPIO to Display** array are logged, but not displayed.

    - Select **SO0 Serializer** from the **GPIO Bank Select** drop down menu.
    
        > The **GPIO Bank Select** control specifies the GPIO bank used for the **GPIO Bank Output**, **GPIO Bank Output Enable** and **GPIO Bank Read** controls during the generation. The **GPIO Bank Select** selection may be changed at runtime.

    - In the **GPIO Bank Output Enable** cluster, enable the **GPIO 0** and **GPIO 3** controls.
        > The **GPIO Bank Output Enable** controls are used as write enables for manual GPIO writes to override the defined GPIO routing. In this case we are choosing to write to lines 0 and 3 of the GPIO selected on the **GPIO Bank Select** control and leave all other lines in the bank unchanged.

2. Run the VI, wait for the **Waiting for Serializer Setup** indicator to illuminate, and click the **Serializer Setup Complete** control button to start the generation.

3. Click **Write GPIO** to write false values to the lines 0 and 3 on the SO 0 Serializer GPIO bank.
    > Notice that the **GPIO 0** and **GPIO 3** Boolean indicators in the **GPIO Bank Read** cluster are not illuminated (false).

4. Set the **GPIO 0** button in the **GPIO Bank Write** control to <font face = "courier new">true</font> then click the **Write GPIO** button to write a true value to line 0 on the SO 0 Serializer GPIO bank.
    > Notice that the **GPIO 0** Boolean indicator in the **GPIO Bank Read** cluster is now illuminated (true).

5. Set the **GPIO 3** button in the **GPIO Bank Write** control to <font face = "courier new">true</font> then click the **Write GPIO** button to write a true value to line 3 on the SO 0 Serializer GPIO bank.
    > Notice that the **GPIO 3** Boolean indicator in the **GPIO Bank Read** cluster is now illuminated (true).

6. Set the **GPIO 0** and **GPIO 3** buttons in the **GPIO Bank Write** control to <font face = "courier new">false</font> then click the **Write GPIO** button to write a false values to lines 0 and 3 on the SO 0 Serializer GPIO bank.
    > Notice that the **GPIO 0** and **GPIO 3** Boolean indicators in the **GPIO Bank Read** cluster are not illuminated (false).

7. Click the **Stop Generation** button.

8. Once the VI stops running, click on the **GPIO Timestamps** tab to view a digital waveform of the selected GPIO line(s).

    > GPIO timestamp data is displayed in the **GPIO Timestamps Waveform** digital waveform indicator. The pattern observed matches the sequence of manual writes that were performed on GPIO lines 0 and 3 during the tutorial.
    
    > Note: The digital waveform is read from the GPIO_Timestamps.tdms file and filtered to display only timestamps for GPIO lines included in the **GPIO to Display** array. The **GPIO Timestamps Waveform** display is updated after the acquisitions completes.

### Routing GPIO Lines

This tutorial section shows you how to define GPIO line routes between GPIO banks on Serial Input/Output channel pairs.
> Note: This tutorial section applies to interface modules with serial input channels and serial output channels only.

> Note: The [Creating a TDMS File for the GPIO Tutorial](#creating-a-tdms-file-for-the-gpio-tutorial) section should be run prior to running the steps in this tutorial.

1. Select the **GPIO** tab and make the following modifications.
    - Add a GPIO route at index 0 of the **GPIO Routes** array with the following settings.
        - Select **SI to SO** in the **Direction** control.
        - Set **Serial Channel** to <font face = "courier new">0</font>.
        - Select **GPIO 0** in the **Source** control.
        - Select **GPIO 3** in the **Destination** control.
    - Add a GPIO route at index 1 of the **GPIO Routes** array with the following settings.
        - Select **SI to SO** in the **Direction** control.
        - Set **Serial Channel** to <font face = "courier new">0</font>.
        - Select **GPIO 1** in the **Source** control.
        - Select **GPIO 0** in the **Destination** control.

    > These settings define the GPIO routes shown below.
    >
    > | From (Source) | To (Destination) |
    > |---|---|
    > | SI 0 Deserializer Line 0 | SO 0 Serializer Line 3|
    > | SI 0 Deserializer Line 1 | SO 0 Serializer Line 0 |

2. Select the **GPIO** tab and make the following modifications.
    - Update the **GPIO to Display** array with the following settings.
        - At index 0 select **Ser0 GPIO** in the **GPIO Bank** control and set the **GPIO Number** to <font face = "courier new">0</font>. 
        - At index 1 select **Ser0 GPIO** in the **GPIO Bank** control and set the **GPIO Number** to <font face = "courier new">3</font>.

        > Setting these values enables display of GPIO traffic for GPIO line 0 and line 3 on the SO0 channel.
        >
        > The **GPIO to Display** control specifies GPIO lines to display on the GPIO Timestamps Waveform after the acquisition completes. Timestamps for GPIO lines not included in the **GPIO to Display** array are logged, but not displayed.

    - Select **SI 0 Deserializer** from the **GPIO Bank Select** drop down menu.

        > The **GPIO Bank Select** control specifies the GPIO bank used for the **GPIO Bank Output**, **GPIO Bank Output Enable** and **GPIO Bank Read** controls during the generation. The **GPIO Bank Select** selection may be changed at runtime.

    - In the **GPIO Bank Output Enable** cluster, enable the **GPIO 0** and **GPIO 1** controls.

        > The **GPIO Bank Output Enable** controls are used as write enables for manual GPIO writes to override the defined GPIO routing. In this case we are choosing to write to lines 0 and 3 of the GPIO selected on the **GPIO Bank Select** control and leave all other lines in the bank unchanged.

2. Run the VI, wait for the **Waiting for Serializer Setup** indicator to illuminate, and click the **Serializer Setup Complete** control button to start the generation.

3. Click **Write GPIO** to write false values to the lines 0 and 1 on the SI 0 Deserializer GPIO bank.
    > Notice that the **GPIO 0** and **GPIO 1** Boolean indicators in the **GPIO Bank Read** cluster are not illuminated (false).

4. Set the **GPIO 0** button in the **GPIO Bank Write** control to <font face = "courier new">true</font> then click the **Write GPIO** button to write a true value to line 0 on the SI 0 Deserializer GPIO bank.
    > Notice that the **GPIO 0** Boolean indicator in the **GPIO Bank Read** cluster is now illuminated (true).

5. Set the **GPIO 1** button in the **GPIO Bank Write** control to <font face = "courier new">true</font> then click the **Write GPIO** button to write a true value to line 1 on the SI 0 Deserializer GPIO bank.
    > Notice that the **GPIO 1** Boolean indicator in the **GPIO Bank Read** cluster is now illuminated (true).

6. Set the **GPIO 0** and **GPIO 1** buttons in the **GPIO Bank Write** control to <font face = "courier new">false</font> then click the **Write GPIO** button to write a false values to lines 0 and 1 on the SI 0 Deserializer GPIO bank.
    > Notice that the **GPIO 0** and **GPIO 1** Boolean indicators in the **GPIO Bank Read** cluster are not illuminated (false).

7. Click the **Stop Generation** button.

8. Once the VI stops running, click on the **GPIO Timestamps** tab to view a digital waveform of the selected GPIO line(s).

    > GPIO timestamp data is displayed in the **GPIO Timestamps Waveform** digital waveform indicator. The waveforms observed are for the destinations of the GPIO routing. Notice that the rising edge transition on SO 0 Serializer Line 3, which was caused by the manual write to SI 0 Deserializer Line 0, occurs before the rising edge transition on SO 0 Serializer Line 0.

    > Note: The digital waveform is read from the GPIO_Timestamps.tdms file and filtered to display only timestamps for GPIO lines included in the **GPIO to Display** array. The **GPIO Timestamps Waveform** display is updated after the acquisitions completes.

## Related Documents
- [PXIe-148X Getting Started Example - Basic Generation Tutorial](./gse-gen-basic.md)
- [PXIe-148X Getting Started Example - Generation Help](../../reference/gettingstartedexample/gse-gen-help.md)
- [PXIe-148X Getting Started Example - Common Acquisition Tutorials](./gse-acq-common.md)
- [PXIe-148X Getting Started Example - Common Tap Tutorials](./gse-tap-common.md)
