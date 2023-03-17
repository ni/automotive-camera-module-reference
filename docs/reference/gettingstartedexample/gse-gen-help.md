# PXIe-148X Getting Started Example - Generation Help
{: .no_toc }

Refer to this document to understand the elements of the getting started example (GSE) for generation with variants of the PXIe-148X GMSL and FPD-Link interface modules. This document provides a description for all controls and indicators on the generation getting started example front panel. 
> Note: Updates to controls when the VI is running will not take effect unless otherwise indicated.

> Note: This document references the example included with the NI-FlexRIO 22Q3 driver. Examples included in newer releases of the driver should be applicable.

### Table of contents
{: .no_toc }

1. TOC
{:toc}

---

## Configuration Settings

### Resource Tab
- **RIO Device** - Resource name of the PXIe-148X module to be used.
- **Bitfile Path** - Full path to an FPGA bitfile (<font face = "courier new">.lvbitx</font>) to be downloaded and run on the FPGA of the PXIe-148X. Depending on your PXIe-148X module, use the following bitfiles.
  #### Table of PXIe-148X Generation Bitfiles

  | **Interface Module**   | **Bitfile**                           |
  |------------------------|---------------------------------------|
  | PXIe-1486 (8 Out)      | <font face = "courier new">PXIe_1486_8\_Out.lvbitx</font>               |
  | PXIe-1486 (4 In 4 Out) | <font face = "courier new">PXIe_1486_4\_In_4\_Out_Gen.lvbitx</font>     |
  | PXIe-1487 (8 Out)      | <font face = "courier new">PXIe_1487_8\_Out.lvbitx</font>               |
  | PXIe-1487 (4 In 4 Out) | <font face = "courier new">PXIe_1487_4\_In_4\_Out_Gen.lvbitx</font>     |

- **TDMS File Directory** - Path to the directory used to load TDMS data files. 
    > If left blank, the TDMS File Directory is automatically populated with a path to a <font face="courier new">TDMS Files</font> subfolder within the getting started example root directory. TDMS data files include files for LLP packet data, I2C timestamps, and GPIO timestamps.
- **Display Generated Images** - Enable to display generated images.
    > Image data is transferred from the FPGA to the host for up to two serial channels by default. Displaying more generated images requires modifying the host and FPGA source code and recompiling the FPGA bitfile.
- **Log I2C to Disk** - Enable to log I2C timestamps to a TDMS file.
- **Log GPIO to Disk** - Enable to log GPIO timestamps to a TDMS file.

### Serial Channel Tab
- **Packet Timing Error Threshold (s)** - Threshold to determine the point at which a difference between the requested time and the actual time a packet is generated is considered an error.
- **Start Trigger Delay (s)** - Delay value that determines the amount of time to wait before asserting the start trigger to begin generation.
    > Note: By default, the starting offset is automatically set to the negative of the first timestamp value read from the file. This causes packet generation to start immediately when the start trigger is asserted. The value of the **Start Trigger Delay (s)** control is added to this negative default value and used to delay the start of the generation; setting the **Start Trigger Delay(s)** control to 0 eliminates any unwanted delay and synchronizes all SO channels.

- **Serializer (Output) Configuration Script** - Full path to a script file used to configure the serializer.
  
- **Channel Configurations** - Array of active serial channels and display parameters for each channel.
    - **Serial Channel** - String representing the active serial channel in the format *SI* for Serial Input or *SO* for Serial Output followed by the channel number (i.e. *SI0*)
    - **FPGA Display Parameters** - Parameters that determine what packet data the FPGA sends to the host for display.
        > Note: Display parameters affect only the image on the display channel, not any data loaded from disk for image generation and transmission.
        
        - **virtual channel** - Number representing a *virtual channel identifier*.
            > Virtual channel identifiers designate separate logical channels for data flows interleaved in the data path.
        - **payload data type** - Data type of the LLP packet payload for long packets.
            > Supported Options: YUV420 8-bit, YUV420 10-bit, Legacy YUV420 8-bit, YUV420 8-bit Chroma Shifted, YUV420 10-bit Chroma Shifted, YUV422 8-bit, YUV422 10-bit, RGB565, RGB666, RGB888, RAW 8, RAW 10, RAW 12, RAW 14, RAW 16.
        - **frame skip count** - Number of frames to skip between displayed frames.
        - **line skip count** - Number of lines to skip between displayed lines.
            > Note: For Bayer-encoded images, even values for line skip count are recommended to prevent skipping all pixels associated with a given color component.
        
            > Note : For YUV420 data types, the data is encoded in line pairs in addition to pixel pairs. As a result, line skip counts are treated as line pair skip counts. For example, a skip count of 1 keeps 2 lines and then skips 2 lines, a skip count of 2 keeps 2 lines and then skips 4 lines, etc.

        - **pixel skip count** - Number of pixels to skip between displayed pixels.
            > Note: For Bayer-encoded images, even pixel skip count values are suggested to prevent skipping all pixels associated with a given color component.
        
            > Note : For YUV data types, the the data is encoded in pixel pairs. As a result, pixel skip counts are treated as pixel pair skip counts. For example, a skip count of 1 keeps 2 pixels and then skips 2 pixels, a skip count of 2 keeps 2 pixels and then skips 4 pixels, etc.

    - **RAW Display Parameters** - Parameters that determine how the raw image data is interpreted for display.
        - **Interpretation** - Describes the actual contents and format of the RAW data to be decoded and displayed.
            > Supported Options: Grayscale, Bayer (default), RGB, YUV422, YUV420, Legacy YUV420
        - **Bayer Parameters** - Additional parameters applicable when interpreting RAW data types as Bayer-encoded data to achieve the correct pattern and color balance.
            - **Pattern** - Specifies the variation of the Bayer encoding pattern to use.
                > Supported Options: GB, GR, BG, RG (default)
            - **Algorithm** - Specifies the algorithm used to create the image. Because the bilinear algorithm is faster, it is recommended to try the bilinear algorithm before the variable number of gradients (VNG) algorithm. If the image contains many edges, or if the quality of the edges in the image is important, use the VNG algorithm.
                > Supported Options: Bilinear (default), VNG
            - **Red Gain** - The gain to be applied to the red pixels in a Bayer-encoded image. The valid range for this parameter is 0 to 3.999.
            - **Green Gain** - The gain to be applied to the green pixels in a Bayer-encoded image. The valid range for this parameter is 0 to 3.999.
            - **Blue Gain** - The gain to be applied to the blue pixels in a Bayer-encoded image. The valid range for this parameter is 0 to 3.999.

### Board Tab
- **Power Over Coax Sink** - The destination used to receive power over coax on active serial output channels.

  | Value     | Description                                                |
  |-----------|------------------------------------------------------------|
  | **None**      | Power over coax disabled.                                  |
  | **Auxiliary** | Route received power over coax to the AUX POWER connector. |

- **Power Over Coax Source** - The source used to provide power over coax on active serial input channels. (Applies to Tap Boards only with [I2C -> Enable I2C TAP (All Channels)](#i2c-tab) enabled)
    
  | Value     | Description                                                         |
  |-----------|---------------------------------------------------------------------|
  | None      | No power over coax provided.                                        |
  | Internal  | Use internal power source to provide power over coax.               |
  | Auxiliary | User power from the AUX POWER connector to provide power over coax. |

- **Reference Clock Frequency (Hz)** - The frequency of the reference clock provided to the reference clock input of the SerDes. GMSL SerDes do not support changing this value from the default of 25 MHz. However for FPD-Link SerDes, in certain modes of operation, you may need to adjust this value and should consult the specifications for your SerDes for more information about supported reference clock frequencies and clocking modes.

- **Requested Serial Output Data Rate (bps)** - Specifies the desired serializer data rate. Passing a requested value of -1 uses the maximum data rate for the board.

- **Actual Serial Output Data Rate (bps)** - The actual data rate set for the serial output channels. The driver might coerce the data rate from the requested rate to a supported value.

- **Serial Output Active Lane Count** - Specifies the number of CSI-2 data lanes for the serializer to use when outputting generated data.

### I2C Tab
- **timestamp filter** - Specifies I2C timestamp IDs to display after the generation completes.  I2C timestamps with IDs that do not match an ID included in the timestamp filter array are logged but not displayed.
    > User24 represents the I2C traffic on serial channel 0 (SI0), User25 represents the I2C traffic on serial channel 1 (SI1), and so on.

- **Enable I2C TAP (All Channels)** - Enables I2C tapping of serial input channels while running a generation bitfile on a PXIe-148X tap board (e.g. 4 In 4 Out). This setting has no effect on PXIe-148x generation boards (e.g. 8 Out).

### GPIO Tab
- **GPIO to Display** - Specifies GPIO timestamp identifiers to display on the *GPIO Timestamps Waveform* after the generation completes. Timestamps for GPIO lines not included in the *GPIO to Display* array are logged but not displayed.
- **GPIO Bank Select** - Specifies the GPIO bank used for the **GPIO Bank Read** and **GPIO Bank Write** controls during the generation.
    > This control may be modified while the VI is running.
- **Update GPIO Output** - Performs a manual GPIO bank write when enabled. 
    > This control may be set while the VI is running.
- **GPIO Bank Output** - Specifies GPIO line values to be written to the selected GPIO bank when **GPIO Bank Output Enable** is set and **Update GPIO Output** is clicked.
    > This control may be modified while the VI is running.
- **GPIO Bank Output Enable** - Enables the specified GPIO line for output to the selected GPIO bank. Setting **GPIO Bank Output Enable** takes priority over routed GPIO lines defined by **GPIO Routing Configuration**. The value of **GPIO Bank Output** overrides an existing line value when **Update GPIO Output** is clicked.
    > This control may be modified while the VI is running.
- **GPIO Bank Read** - Displays GPIO line values read from the selected GPIO bank.
    > **GPIO Bank Read** line values are updated continuously throughout the generation.
- **GPIO Routing Configuration** - Specifies GPIO routing configurations to route GPIO lines through the FPGA between serial input and output channel pairs (SO0/SI0, SO1/SI1, SO2/SI2, SO3/SI3) on a PXIe-148X tap board (e.g. 4 In 4 Out). This setting has no effect on PXIe-148x generation boards (e.g. 8 Out).
     > This control may be modified while the VI is running.

## Data Output
### Display Channel Tab (First or Second)
> By default, the getting started example allows displaying images for two serial input channels. The images can be viewed by clicking on the First Display Channel and Second Display Channel tabs while the generation is running.

- **Image Display** - Displays the most recently generated frame of the corresponding display channel. Interactive pan and zoom capabilities are provided by the buttons in the upper left corner of the image display. The text field below the image shows the image size, zoom factor, image type, pixel value and coordinate information.
- **Serial Channel** - Selects the serial channel number for the image data to be displayed if more than two serial input channels are active.
    > This control may be modified while the VI is running.
- **Source Rate (fps)** - Displays the frame rate, in frames per second, at which the image data is being transmitted.
- **Update Rate (fps)** - Displays the rate, in frames per second, at which the image display indicator is being updated.
    > Note: If multiple frames have been generated since the last update, only the most recent frame is displayed, and the others are skipped in an attempt to keep up with the generation.

### I2C Timestamps Tab
> Note: To display I2C timestamp data, **Log I2C to Disk** must be enabled on the **Resource** tab and desired timestamp IDs must be added to the **timestamp filter** array on the **I2C** tab. The I2C timestamp data displayed is read from the <font face="courier new">User_Timestamps.tdms</font> file and filtered to display only the timestamp IDs included in the **timestamp filter** array.

- **I2C Timestamps** - Displays I2C timestamp information for all I2C IDs in the **timestamp filter** array on the **I2C** tab. I2C timestamps begin logging immediately after the FPGA bitfile is downloaded and include timestamp data prior to the start of the LLP packet data generation, such as I2C traffic from a configuration script.
    > The **I2C Timestamps** display is updated after the generation completes.

### GPIO Timestamps Tab
> Note: To display GPIO digital waveforms, **Log GPIO to Disk** must be enabled on the **Resource** tab and desired GPIO lines must be added to the **GPIO to Display** array on the **GPIO** tab. The GPIO timestamp data displayed is read from the <font face="courier new">GPIO_Timestamps.tdms</font> file and filtered to display only GPIO lines included in the **GPIO to Display** array.

- **GPIO Timestamps Waveform** - Displays a digital waveform for each GPIO line included in the **GPIO to Display** array on the **GPIO** tab. GPIO timestamps begin logging immediately after the FPGA bitfile is downloaded and include timestamp data prior to the start of the LLP packet data generation, such as toggles created from a reset.
    > The **GPIO Timestamps Waveform** display is updated after the generation completes.
- **Error Parsing GPIO Timestamps** - Indicates whether an error occurred while creating the digital waveform in the <font face="courier new">Build GPIO Waveform.vi</font> SubVI.

## General
- **Stop Generation** - Creates a manual stop trigger for the generation. Clicking this button stops all processing loops (Display, I2C, GPIO, monitoring, and LLP generation) and causes the VI to stop running.
    > This control may be set while the VI is running.
- **Generation State** - Displays the current state of the generation (Idle, Ready for Start Trigger, Generate Packets, Done, or Error)
- **Generation Stopped** - Indicates that the generation has stopped. Generation can stop upon clicking **Stop Generation**, when an error condition is detected, or upon completing successfully.
- **Number of Generated Packets** - Displays the total number of LLP packets generated for each serial output channel.
- **Packet Timing Errors** - Indicates whether a packet timing error was detected for each serial output channel, as dictated by the value of the **Packet Timing Error Threshold (s)** on the **Serial Channel** tab.
- **Serializer Setup Complete** - Starts the process of buffering data to onboard DRAM and generation. Click this button after **Waiting for Serializer Setup** is illuminated and serializer setup through a connected acquisition device (e.g. an ECU or PXIe-148X acquisition board) has completed.
- **Waiting for Serializer Setup** - Indicates that the board is initialized, ready for serializer setup by a connected acquisition device, and paused before starting generation to allow serializer setup to complete.
    > Note: **Serializer Setup Complete** and **Waiting for Serializer Setup** only apply when no script is defined in **Serializer (Output) Configuration Script** on the **Serial Channel** tab. If a serializer configuration script is defined, the VI does not pause for external serializer setup.
- **Power Over Coax Current (A)** - Displays the power over coax current draw, in amps, for all active serial output channels.
- **Power Over Coax Voltage (V)** - Displays the power over coax voltage level, in volts, for all active serial output channels.
- **Power Over Coax Sink In Range** - Indicates whether the power over coax voltage level is in range for all active serial output channels.
- **Power Over Coax Source In Range** - Indicates if the power over coax voltage level is in range for all active serial input channels. (Applies to Tap Boards only with [I2C -> Enable I2C TAP (All Channels)](#i2c-tab) enabled)
- **Error Out** - Displays any error that occurred during the generation.

## Related Documents
- [PXIe-148X Getting Started Example - Basic Generation Tutorial](../../tutorials/gettingstartedexample/gse-gen-basic.md)
- [PXIe-148X Getting Started Example - Common Generation Tutorials](../../tutorials/gettingstartedexample/gse-gen-common.md)
- [PXIe-148X Getting Started Example - Acquisition Help](./gse-acq-help.md)
- [PXIe-148X Getting Started Example - Tap Help](./gse-tap-help.md)
