# PXIe-148X Getting Started Example - Tap Help
{: .no_toc }

Refer to this document to understand the elements of the getting started example for tap with variants of the PXIe-148X GMSL and FPD-Link interface modules. This document provides a description for all controls and indicators on the tap getting started example front panel. 
> Note: Updates to controls when the VI is running will not take effect unless otherwise indicated.

> Note: This document references the example included with the NI-FlexRIO 23Q1 driver. Examples included in newer releases of the driver should be applicable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Configuration Settings

### Resource Tab
- **RIO Device** - Resource name of the PXIe-148X device to be used.
- **Bitfile Path** - Full path to an FPGA bitfile (.lvbitx) to be downloaded and run on the FPGA of the PXIe-148X. Depending on your PXIe-148X module, use the following bitfiles.
  #### Table of PXIe-148X Tap Bitfiles

  | **Interface Module**     | **Bitfile**                           |
  |--------------------------|---------------------------------------|
  | PXIe-1486 (4 In 4 Out)   | PXIe_1486_4\_In_4\_Out_Acq_Tap.lvbitx |
  | PXIe-1487 (4 In 4 Out)   | PXIe_1487_4\_In_4\_Out_Acq_Tap.lvbitx |
  | PXIe-1488 (4 In 4 Out)   | PXIe_1488_4\_In_4\_Out_Acq_Tap.lvbitx |
  | PXIe-1489 (2 In 2 Out)   | PXIe_1489_2\_In_2\_Out_Acq_Tap.lvbitx |

- **TDMS File Directory** - Path to the directory used to store TDMS data files. 
    > If left blank the TDMS File Directory will be automatically populated with a path to a subfolder ("TDMS Files") within the getting started example root directory. TDMS data files include files for LLP packet acquisition, I2C timestamps, and GPIO timestamps.
- **Display Acquired Images** - Enable to display acquired images.
    > Image data is transferred from the FPGA to the host for up to two serial channels by default. Displaying more acquired images requires modifying the host and FPGA source code and recompiling the FPGA bitfile.
- **Log Packets to Disk** - Enable to log LLP packet data for each active serial channel to a TDMS file.
- **Logged Packets to Display** - Number of logged LLP packets to display per channel in the _Acquired Packets_ table when the getting started example completes. Specifying a negative number displays all logged packets.
    > Note: Specifying a large number of packets to display may cause the VI to appear unresponsive for a period of time after the acquisition completes while the packet data is processed.
- **Log I2C to Disk** - Enable to log I2C timestamps to a TDMS file.
- **Log GPIO to Disk** - Enable to log GPIO timestamps to a TDMS file.

### Serial Channel Tab
- **CSI-2 Data Source** - Selects the CSI-2 data source type. "Corrected" returns the received data after correcting for single-bit transmission errors. "Raw" returns the received data as is. 
- **Deserializer (Input) Configuration Script** - Full path to a script file used to configure the deserializer.
  #### Table of PXIe-148X Deserializer Tap Scripts

  | **Interface Module**                         | **Configuration Script**                              |
  |----------------------------------------------|-------------------------------------------------------|
  | PXIe-1486 (4 In 4 Out - 953/954 SerDes)      | Host\\Scripts\\DS90UB954\\Tap\\Des_Tap.py             |
  | PXIe-1487 (4 In 4 Out - 9295A/9296A SerDes)  | Host\\Scripts\\MAX9296A\\Tap\\RAW12_ID01_Des_Tap.cpp  |
  | PXIe-1487 (4 In 4 Out - 96717/96716A SerDes) | Host\\Scripts\\MAX96716A\\Tap\\RAW12_ID12_Des_Tap.cpp |
  | PXIe-1488 (4 In 4 Out - 971/9702 SerDes)     | Host\\Scripts\\DS90UB9702\\Tap\\Des_Tap.py            |
  | PXIe-1489 (2 In 2 Out - 96793/96792A SerDes) | Host\\Scripts\\MAX96792A\\Tap\\RAW12_ID12_Des_Tap.cpp |

- **Serializer (Output) Configuration Script** - Full path to a script file used to configure the serializer.
  #### Table of PXIe-148X Serializer Tap Scripts

  | **Interface Module**                                              | **Configuration Script**                                       |
  |-------------------------------------------------------------------|----------------------------------------------------------------|
  | PXIe-1486 (4 In 4 Out - 953/954 SerDes)                           | Host\\Scripts\\DS90UB953\\Tap\\Ser_Tap.py                      |
  | PXIe-1487 (4 In 4 Out - 9295A/9296A SerDes)                       | Host\\Scripts\\MAX9295A\\Tap\\RAW12_ID1_Ser_Tap.cpp            |
  | PXIe-1487 (4 In 4 Out - 9295A/9296A SerDes) Reverse Splitter Mode | Host\\Scripts\\MAX9295A\\Tap\\RAW12_ID0_ToRevSplit_Ser_Tap.cpp |
  | PXIe-1487 (4 In 4 Out - 96717/96716A SerDes)                      | Host\\Scripts\\MAX96717\\Tap\\RAW12_ID2_Ser_Tap.cpp            |
  | PXIe-1488 (4 In 4 Out - 971/9702 SerDes)                          | Host\\Scripts\\DS90UB971\\Tap\\Ser_Tap.py                      |
  | PXIe-1489 (2 In 2 Out - 96793/96792A SerDes)                      | Host\\Scripts\\MAX96793\\Tap\\RAW12_ID2_Ser_Tap.cpp            |

- **Channel Configurations** - Array of active serial channel tap pairs and display parameters for each channel pair.
    - **Serial Channel Tap Pair**
        - **input** - String representing the active serial input channel in the tap pair in the format "SI" for Serial Input followed by the channel number (i.e. "SI0")
        - **output** - String representing the active serial output channel in the tap pair in the format "SO" for Serial Output followed by the channel number (i.e. "SO0")
        > Note: The input and output numeric values in each channel pair must match.
    - **FPGA Display Parameters** - Parameters that determine what packet data the FPGA will send to the host for display.
        > Note: Display parameters only affect the image on the display channel, not any data logged to disk.
        
        - **virtual channel** - Number representing a Virtual Channel Identifier.
            > Virtual channel identifiers designate separate logical channels for data flows interleaved in the data path.
        - **payload data type** - Data type of the LLP packet payload for long packets.
            > Supported Options: YUV420 8-bit, YUV420 10-bit, Legacy YUV420 8-bit, YUV420 8-bit Chroma Shifted, YUV420 10-bit Chroma Shifted, YUV422 8-bit, YUV422 10-bit, RGB565, RGB666, RGB888, RAW 8, RAW 10, RAW 12, RAW 14, RAW 16.
        - **frame skip count** - Number of frames to skip between displayed frames.
        - **line skip count** - Number of lines to skip between displayed lines.
            > Note: For Bayer encoded images, even line skip count values are suggested to prevent skipping all pixels associated with a given color component.
        
            > Note : For YUV420 data types, the the data is encoded in line pairs in addition to pixel pairs so line skip counts are treated as line pair skip counts. For example, a skip count of 1 will keep 2 lines and then skip 2 lines, a skip count of 2 will keep 2 lines and then skip 4 lines, etc.

        - **pixel skip count** - Number of pixels to skip between displayed pixels.
            > Note: For Bayer encoded images, even pixel skip count values are suggested to prevent skipping all pixels associated with a given color component.
        
            > Note : For YUV data types, the the data is encoded in pixel pairs so pixel skip counts are treated as pixel pair skip counts. For example, a skip count of 1 will keep 2 pixels and then skip 2 pixels, a skip count of 2 will keep 2 pixels and then skip 4 pixels, etc.

    - **RAW Display Parameters** - Parameters that determine how the raw image data is interpreted for display.
        - **Interpretation** - Describes the actual contents and format of the RAW data to be decoded and displayed.
            > Supported Options: Grayscale, Bayer (default), RGB, YUV422, YUV420, Legacy YUV420

            > This control may be modified while the VI is running.
        - **Bayer Parameters** - Additional parameters applicable when interpreting RAW data types as Bayer-encoded data to achieve the correct pattern and color balance.
            - **Pattern** - Specifies the variation of the Bayer encoding pattern to use.
                > Supported Options: GB, GR, BG, RG (default)

                > This control may be modified while the VI is running.
            - **Algorithm** - Specifies the algorithm to use to create the image. Because the bilinear algorithm is faster, it is recommended to try the bilinear algorithm before the variable number of gradients (VNG) algorithm. If the image contains many edges, or if the quality of the edges in the image is important use the VNG algorithm.
                > Supported Options: Bilinear (default), VNG

                > This control may be modified while the VI is running.
            - **Red Gain** - The gain to be applied to the red pixels in a Bayer-encoded image. The valid range for this parameter is 0 to 3.999.
                > This control may be modified while the VI is running.
            - **Green Gain** - The gain to be applied to the green pixels in a Bayer-encoded image. The valid range for this parameter is 0 to 3.999.
                > This control may be modified while the VI is running.
            - **Blue Gain** - The gain to be applied to the blue pixels in a Bayer-encoded image. The valid range for this parameter is 0 to 3.999.
                > This control may be modified while the VI is running.

### Acquisition Tab
- **Acquisition Duration (s)** - The acquisition duration in seconds. The duration specifies the time from the start of the acquisition to the assertion of a stop trigger.
    > Note: If continuous acquisition is enabled, the acquisition duration is ignored.

    > Note: The acquisition duration may be longer than specified if **Enable First and Last Packet Select** is enabled since it will wait for a matching data type to be received after the stop trigger is received.
- **Continuous Acquisition** - If enabled the acquisition runs continuously until a stop trigger is manually created using the Stop Acquisition button or an error occurs. If disabled, the stop trigger is automatically generated after the specified **Acquisition Duration (s)** time has elapsed.
- **First Packet Data Type Select** - Specifies the packet type to match before accepting data. All packets received prior to the first packet of the specified type are ignored. 
    > Note: If **Enable First and Last Packet Select** is disabled, **First Packet Data Type Select** is ignored. 
- **Last Packet Data Type Select** - Specifies the packet type to match before ending the acquisition. After a stop trigger is received, the acquisition waits until a packet of the specified type is received or the **Stop Timeout (s)** duration is reached before ending the acquisition.
    > Note: If **Enable First and Last Packet Select** is disabled, **Last Packet Data Type Select** is ignored. 
- **Enable First and Last Packet Select** - If enabled, the first and last packet data type selections are used, otherwise the first and last packet data type selections are ignored.
- **Stop Timeout (s)** - The time in seconds to wait, after a stop trigger is received, for a packet that has a data type corresponding to **Last Packet Data Type Select** before ending the acquisition.
- **Data Type Filter** - Specified packet data types to ignore (filter out) during the acquisition.
- **Invert Data Type Filter** - If enabled, packet data types included in the data type filter array are the only packets acquired and all other packets are ignored. If disabled, packet data types included in the data type filter array are ignored and all other packet data types are acquired.
- **Virtual Channel Filter** - Specifies virtual channel packets to ignore (filter out) during the acquisition.
- **Invert Virtual Channel Filter** - If enabled, packets with a virtual channel included in the virtual channel filter array are the only virtual channel packets acquired and all other virtual channel packets are ignored. If disabled, packets with a virtual channel included in the virtual channel filter array are ignored and all other virtual channel packets are acquired.

### Board Tab
- **Power Over Coax Source** - The source used to provide power over coax on active serial input channels.
    
  | Value     | Description                                                         |
  |-----------|---------------------------------------------------------------------|
  | None      | No power over coax provided.                                        |
  | Internal  | Use internal power source to provide power over coax.               |
  | Auxiliary | Use power from the AUX POWER connector to provide power over coax.  |

- **Power Over Coax Sink** - The destination used to receive power over coax on active serial output channels.

  | Value     | Description                                                |
  |-----------|------------------------------------------------------------|
  | None      | Power over coax disabled.                                  |
  | Auxiliary | Route received power over coax to the AUX POWER connector. |

- **Serial Input Reference Clock Frequency (Hz)** - The frequency of the reference clock provided to the reference clock input of the deserializer. GMSL deserializers do not support changing this from the default value of 25 MHz. However for FPD-Link deserializers, in certain modes of operation, you may need to adjust this value and should consult the datasheet of your deserializer for more information about supported reference clock frequencies and clocking modes.

- **Serial Output Reference Clock Frequency (Hz)** - The frequency of the reference clock provided to the reference clock input of the serializer. GMSL serializers do not support changing this from the default value of 25 MHz. However for FPD-Link serializers, in certain modes of operation, you may need to adjust this value and should consult the datasheet of your serializer for more information about supported reference clock frequencies and clocking modes.

- **Requested Serial Output Data Rate (bps)** - Specifies the desired serializer data rate. Passing a requested value of -1 uses the maximum data rate for the board.

- **Actual Serial Output Data Rate (bps)** - The actual data rate set for the serial output channels. The driver might coerce the data rate from the requested rate to a supported value.

- **Serial Output Active Lane Count** - Specifies the number of CSI-2 data lanes for the serializer to use when outputting data.

- **Serial Input Active Lane Count** - Specifies the number of CSI-2 data lanes for the deserializer to use when acquiring data.

### I2C Tab
- **timestamp filter** - Specifies I2C timestamp IDs to display after the acquisition completes.  I2C timestamps with IDs not matching an ID included in the timestamp filter array are logged, but not displayed.
    > User24 represents the I2C traffic on serial channel 0 (SI0), User25 represents the I2C traffic on serial channel 1 (SI1), and so on.

### GPIO Tab
- **GPIO to Display** - Specifies GPIO timestamp identifiers to display on the *GPIO Timestamps Waveform* after the acquisition completes. Timestamps for GPIO lines not included in the *GPIO to Display* array are logged but not displayed.
- **GPIO Bank Select** - Specifies the GPIO bank used for the **GPIO Bank Read** and **GPIO Bank Write** controls during the acquisition.
    > This control may be modified while the VI is running.
- **Update GPIO Output** - Performs a manual GPIO bank write when enabled. 
    > This control may be set while the VI is running.
- **GPIO Bank Output** - Specifies GPIO line values to be written to the selected GPIO bank when **GPIO Bank Output Enable** is set and **Update GPIO Output** is clicked.
    > This control may be modified while the VI is running.
- **GPIO Bank Output Enable** - Enables the specified GPIO line for output to the selected GPIO bank. Setting **GPIO Bank Output Enable** takes priority over routed GPIO lines defined by **GPIO Routing Configuration**. The value of **GPIO Bank Output** overrides an existing line value when **Update GPIO Output** is clicked.
    > This control may be modified while the VI is running.
- **GPIO Bank Read** - Displays GPIO line values read from the selected GPIO bank.
    > **GPIO Bank Read** line values are updated continuously throughout the acquisition.
- **GPIO Routing Configuration** - Specifies GPIO routing configurations to route GPIO lines through the FPGA between serial input and output channel pairs (SO0/SI0, SO1/SI1, SO2/SI2, SO3/SI3).
     > This control may be modified while the VI is running.

## Data Output

### Display Channel Tab (First or Second)
> By default, the getting started example allows displaying images for two serial input channels, which can be viewed by clicking on the First Display Channel and Second Display Channel tabs while the acquisition is running.

- **Image Display** - Displays the most recently acquired frame of the corresponding display channel. Interactive pan and zoom capabilities are provided by the buttons in the upper left corner of the image display. The text field below the image shows the image size, zoom factor, image type, pixel value and coordinate information.
- **Serial Channel** - Selects the serial channel number for the image data to be displayed if more than two serial input channels are active.
    > This control may be modified while the VI is running.
- **Source Rate (fps)** - Displays the frame rate in frames per second at which the image data is being received.
- **Update Rate (fps)** - Displays the rate in frames per second at which the image display indicator is being updated.
    > Note: If multiple frames have arrived since the last update, only the most recent frame is displayed and the others are skipped in an attempt to keep up with the acquisition.

### Serial Channel Packets Tab (First or Second)
> The getting started example displays packet data for the first two channels specified in the Channel Configurations array at the completion of the acquisition. If other packet data is desired, either re-order the active channels in the configuration array on the Serial Channel tab or use the TDMS File Viewer utility.

> Note: If displaying a large number of packets, the VI may appear unresponsive for a period of time after the acquisition completes while the packet data is processed.

- **Acquired Packets** - Displays the LLP packet information for the number of packets specified in the **Logged Packets to Display** control starting at the first logged packet.
  #### Table of Descriptions for Acquired Packets Columns

  | Column | Description |
  | ------ | ----------- |
  | Index | The order packets were received. |
  | Timestamp (s) | The time in seconds when the packet was received. Timestamps are relative to a time immediately after the FPGA bitfile is downloaded and run, not the start of the acquisition. This allows capturing I2C and GPIO timestamps during configuration before the acquisition starts. |
  | Source | The unique timestamp identifier that indicates the source serial channel for the LLP packet. |
  | Data Type | The type of data contained in the LLP packet (e.g. Frame Start, Frame End, RAW types, YUV types, RGB types, etc.). |
  | Virtual Channel | A number representing a virtual channel identifier. Virtual channel identifiers designate separate logical channels for data flows interleaved in the data path. |
  | ECC | The Error Correction Code (ECC) detects bit errors in the header. |
  | Word Count | The number of bytes in a long packet (i.e. payload size or bytes per line) and does not include the header or footer bytes. |
  | Line Count | The Line Count value comes from the Word Count field in Line Start and Line End packets. |
  | Frame Count | The Frame Count value comes from the Word Count field in Frame Start and Frame End packets, but there are no requirements on what the value is set to. |
  | CRC | The Cyclic Redundancy Check (CRC) comes from the footer of every long packet and is used to determine the validity of the long packet payload data. |

- **Bytes Acquired** - Displays the total number of bytes of LLP packet data acquired during the acquisition.
    > The **Bytes Acquired** indicator updates during the acquisition.
- **Packets Logged** - Displays the total number of LLP packets logged during the acquisition.
    > The **Packets Logged** indicator updates after the acquisition completes.

### I2C Timestamps Tab
> Note: To display I2C timestamp data, **Log I2C to Disk** must be enabled on the Resource tab and desired timestamp IDs must be added to the **timestamp filter** array on the I2C tab. The I2C timestamp data displayed is read from the User_Timestamps.tdms file and filtered to display only timestamp IDs included in the **timestamp filter** array.

- **I2C Timestamps** - Displays I2C timestamp information for all I2C IDs in the **timestamp filter** array on the I2C tab. I2C timestamps begin logging immediately after the FPGA bitfile is downloaded and include timestamp data prior to the start of the LLP packet data acquisition, such as I2C traffic from the configuration script.
    > The **I2C Timestamps** display is updated after the acquisition completes.

### GPIO Timestamps Tab
> Note: To display GPIO digital waveforms, **Log GPIO to Disk** must be enabled on the Resource tab and desired GPIO lines must be added to the **GPIO to Display** array on the GPIO tab. The GPIO timestamp data displayed is read from the GPIO_Timestamps.tdms file and filtered to display only GPIO lines included in the **GPIO to Display** array.

- **GPIO Timestamps Waveform** - Displays a digital waveform for each GPIO line included in the **GPIO to Display** array on the GPIO tab. GPIO timestamps begin logging immediately after the FPGA bitfile is downloaded and include timestamp data prior to the start of the LLP packet data acquisition, such as toggles created from a reset.
    > The **GPIO Timestamps Waveform** display is updated after the acquisition completes.
- **Error Parsing GPIO Timestamps** - Indicates if an error occurred while creating the digital waveform in the <font face="courier new">Build GPIO Waveform.vi</font> SubVI.

## General
- **Sensor Setup Complete** - Starts the tap process after SerDes configuration has completed. Click this button after **Waiting for Sensor Setup** is illuminated and SerDes setup through a connected acquisition device (e.g. an ECU or PXIe-148X acquisition board) has completed.
- **Waiting for Sensor Setup** - Indicates that the board is initialized, ready for SerDes setup by a connected acquisition device, and paused before starting tap to allow SerDes setup to complete.
- **Stop Acquisition** - Creates a manual stop trigger for the acquisition. Clicking this button will cause all processing loops (Display, I2C, GPIO, monitoring, and LLP acquisition) to stop and the VI to stop running.
    > This control may be set while the VI is running.\
\
The **Power Over Coax Source** is set to `None` and the call to [Wait for Safe to Disconnect](../hardware/powerovercoax.md#wait-for-safe-to-disconnect) may take several seconds while the channel discharges and the VI may appear unresponsive. The timeout is set to a constant 60 s on the block diagram and may need adjustment depending on the details of the system. If using an external power source, turn it off before the call to `Wait for Safe to Disconnect`.\
*Updated in version NI-FlexRIO 24Q1*

- **Acquisition In Progress** - Indicates that the configuration is complete and the acquisition has started.
- **Acquisition Duration (s)** - Displays the duration in seconds between the start of the acquisition and the time a stop trigger is received.
    > Note: **Acquisition Duration (s)** does not include configuration time or post processing time.
- **Power Over Coax Current (A)** - Displays the power over coax current draw in amps for all active serial input channels.
- **Power Over Coax Voltage (V)** - Displays the power over coax voltage level in volts for all active serial input channels.
- **Power Over Coax Source In Range** - Indicates if the power over coax voltage level is in range for all active serial input channels.
- **Power Over Coax Sink In Range** - Indicates whether the power over coax voltage level is in range for all active serial output channels.
- **Error Out** - Displays any error that occurred during the acquisition.

## Related Documents
- [PXIe-148X Getting Started Example - Basic Tap Tutorial](../../tutorials/gettingstartedexample/gse-tap-basic.md)
- [PXIe-148X Getting Started Example - Common Tap Tutorials](../../tutorials/gettingstartedexample/gse-tap-common.md)
- [PXIe-148X Getting Started Example - Acquisition Help](./gse-acq-help.md)
- [PXIe-148X Getting Started Example - Tap Help](./gse-tap-help.md)
