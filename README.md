# PSoC 4: MSC Self-Capacitance Touchpad Tuning

**Alpha Release Content** - Support for PSoC 4 family devices on ModusToolbox is currently in the alpha stage. Features may change without notice. Contact [Support](https://www.cypress.com/support) for additional details.

This code example demonstrates how to manually tune a self-capacitance-based Touchpad widget in PSoC® 4 devices using the Multi Sense Converter (MSC) CSD-RM sensing technique and CapSense® Tuner GUI. Here, CapSense Sigma Delta (CSD) represents the self-capacitance sensing technique and RM represents the ratiometric method.

[Provide feedback on this Code Example.](https://cypress.co1.qualtrics.com/jfe/form/SV_1NTns53sK2yiljn?Q_EED=eyJVbmlxdWUgRG9jIElkIjoiQ0UyMzIyNzMiLCJTcGVjIE51bWJlciI6IjAwMi0zMjI3MyIsIkRvYyBUaXRsZSI6IlBTb0MgNDogTVNDIFNlbGYtQ2FwYWNpdGFuY2UgVG91Y2hwYWQgVHVuaW5nIiwicmlkIjoic2FoZyIsIkRvYyB2ZXJzaW9uIjoiMS4wLjAiLCJEb2MgTGFuZ3VhZ2UiOiJFbmdsaXNoIiwiRG9jIERpdmlzaW9uIjoiTUNEIiwiRG9jIEJVIjoiSUNXIiwiRG9jIEZhbWlseSI6IlBTT0MifQ==)

## Requirements

- [ModusToolbox® software](https://www.cypress.com/products/modustoolbox-software-environment) v2.2

  **Note:** This code example version requires ModusToolbox software version 2.2 and is not backward compatible with v2.1 or older versions.

- Board Support Package (BSP) minimum required version: 0.5.0 
- Programming Language: C
- Associated Parts: [PSoC 4100S Max](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4100s-max-datasheet-programmable-system-chip-psoc)

## Supported Toolchains (make variable 'TOOLCHAIN')

- GNU Arm® Embedded Compiler v9.3.1 (`GCC_ARM`) - Default value of `TOOLCHAIN`
- Arm compiler v6.13 (`ARM`)
- IAR C/C++ compiler v8.42.2 (`IAR`)

## Supported Kits (make variable 'TARGET')

- [CY8CKIT-041S-MAX PSoC 4100S Max Pioneer Kit](https://www.cypress.com/documentation/development-kitsboards/psoc-4100s-max-pioneer-kit-cy8ckit-041s-max) (`CY8CKIT-041S MAX`) - Default target

## Hardware Setup

This example uses the board's default configuration. See the [kit user guide](https://www.cypress.com/file/520036/download) to ensure that the board is configured correctly to VDDA at 5V (J10 should be at position 1 and 2). If you are using the code example at a VDDA voltage other than 5 V, ensure to set up the device power voltages correctly for the proper operation of the device power domains.  See [Steps to Setup the VDDA Supply Voltage in Device Configurator](#steps-to-setup-the-vdda-supply-voltage-in-device-configurator) for more details.

## Software Setup

This example requires no additional software or tools.

## Using the Code Example

Create the project and open it using one of the following:

<details><summary><b>In Eclipse IDE for ModusToolbox</b></summary>

1. Click the **New Application** link in the **Quick Panel** (or, use **File** > **New** > **ModusToolbox Application**). This launches the [Project Creator](http://www.cypress.com/ModusToolboxProjectCreator) tool.

2. Pick a kit supported by the code example from the list shown in the **Project Creator - Choose Board Support Package (BSP)** dialog.

   When you select a supported kit, the example is reconfigured automatically to work with the kit. To work with a different supported kit later, use the [Library Manager](https://www.cypress.com/ModusToolboxLibraryManager) to choose the BSP for the supported kit. You can use the Library Manager to select or update the BSP and firmware libraries used in this application. To access the Library Manager, click the link from the **Quick Panel**.

   You can also just start the application creation process again and select a different kit.

   If you want to use the application for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work.

3. In the **Project Creator - Select Application** dialog, choose the example by enabling the checkbox.

4. Optionally, change the suggested **New Application Name**.

5. Enter the local path in the **Application(s) Root Path** field to indicate where the application needs to be created.

   Applications that can share libraries can be placed in the same root path.

6. Click **Create** to complete the application creation process.

For more details, see the [Eclipse IDE for ModusToolbox User Guide](https://www.cypress.com/MTBEclipseIDEUserGuide) (locally available at *{ModusToolbox install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*).

</details>

<details><summary><b>In Command-line Interface (CLI)</b></summary>

ModusToolbox provides the Project Creator as both a GUI tool and a command line tool to easily create one or more ModusToolbox applications. See the "Project Creator Tools" section of the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) for more details.

Alternatively, you can manually create the application using the following steps:

1. Download and unzip this repository onto your local machine, or clone the repository.

2. Open a CLI terminal and navigate to the application folder.

   On Linux and macOS, you can use any terminal application. On Windows, open the **modus-shell** app from the Start menu.

   **Note:** The cloned application contains a default BSP file (*TARGET_xxx.mtb*) in the *deps* folder. Use the [Library Manager](https://www.cypress.com/ModusToolboxLibraryManager) (`make modlibs` command) to select and download a different BSP file, if required. If the selected kit does not have the required resources or is not [supported](#supported-kits-make-variable-target), the application may not work.

3. Import the required libraries by executing the `make getlibs` command.

Various CLI tools include a `-h` option that prints help information to the terminal screen about that tool. For more details, see the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox install directory}/docs_{version}/mtb_user_guide.pdf*).

</details>

<details><summary><b>In Third-party IDEs</b></summary>

1. Follow the instructions from the **In Command-line Interface (CLI)** section to create the application, and import the libraries using the `make getlibs` command.

2. Export the application to a supported IDE using the `make <ide>` command.

   For a list of supported IDEs and more details, see the "Exporting to IDEs" section of the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox install directory}/docs_{version}/mtb_user_guide.pdf*.

3. Follow the instructions displayed in the terminal to create or import the application as an IDE project.

</details>

The project already has the necessary settings by default, so you can go to [Test the Basic Operation](#test-the-basic-operation) to verify the operation. If you want to understand the tuning process and follow the stages for this kit or your own board, go to [Tuning Procedure](#tuning-procedure) and then test it using [Test the Basic Operation](#test-the-basic-operation).

## Overview

This document includes: 

- A testing section to showcase the performance of a CSD-RM touchpad widget to track a single finger.

- A high-level overview of the CSD-RM touchpad widget tuning flow. 

- A procedure that explains how to use the CapSense Tuner to monitor the CapSense raw data and fine-tune the CSD-RM touchpad for optimum performance in parameters such as response time and linearity.

## Test the Basic Operation

1. Power the device by plugging a USB 2.0 Type A to Micro-B cable on J8 (USB Micro-B connector). 
      
   **Figure 1. Connecting the CY8CKIT-041S-MAX Kit with CapSense Expansion Board to a Computer**

   <img src="images/connection.png" alt="Figure 1" width="500"/>

2. Program the board using one of the following:

   <details><summary><b>Using Eclipse IDE for ModusToolbox</b></summary>

      1. Select the application project in the Project Explorer.

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3_MiniProg4)**.
      </details>

   <details><summary><b>Using CLI</b></summary>

     From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. You can specify a target and toolchain manually:
      ```
      make program TARGET=<BSP> TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TARGET=CY8CPROTO-062-4343W TOOLCHAIN=GCC_ARM
      ```
</details>

3. After programming, the application starts automatically.

4. To test the application, slide your finger over the CapSense Touchpad and notice that the user LED turns ON when touched and turns OFF when the finger is lifted.

5. You can also monitor the CapSense data using the CapSense Tuner application as follows:

    **Monitor Data Using CapSense Tuner**

    1. Open CapSense Tuner from the Tools section in the IDE Quick Panel. 
    
        You can also run the CapSense Tuner application standalone from *{ModusToolbox install directory}/ModusToolbox/tools_{version}/capsense-configurator/capsense-tuner*. In this case, after opening the application, select **File** > **Open** and open the *design.cycapsense* file of the respective application, which is present in the *{Application root directory}/COMPONENT_CUSTOM_DESIGN_MODUS/TARGET_\<BSP-NAME>* folder. 

	     See the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox install directory}/docs_{version}/mtb_user_guide.pdf*) for options to open the CapSense Tuner Application using the CLI.

    2. Ensure the kit is in CMSIS-DAP bulk mode (KitProg3 Status LED is ON and not blinking). See [Firmware-loader](https://github.com/cypresssemiconductorco/Firmware-loader) to learn on how to update the firmware and switch modes in KitProg3.
  
    3. In the Tuner application, click on the **Tuner Communication Setup** icon or Select **Tools** > **Tuner Communication Setup**. In the window that appears, select the I2C checkbox under KitProg3 and configure as follows: 

       - **I2C address: 8**
       - **Sub-address: 2-Bytes**
       - **Speed (kHz): 1000**

        These are the same values set in the EZI2C resource.

        **Figure 2. Tuner Communication Setup Parameters**

        <img src="images/tuner_setup.png" alt="Figure 2" width="500" />

    4. Click **Connect** or select **Communication** > **Connect** to establish a connection.

    5. Click **Start** or select **Communication** > **Start** to start data streaming from the device. 
   
       The tuner displays the data from the sensor in the **Widget View**, **Graph View**, and **Touchpad View** tabs. 

    6. Set the **Read Mode** to Synchronized mode. Under the **Widget View** tab, you can see the Touchpad widget highlighted in blue color when you touch it. 

       **Figure 3. Widget View of the CapSense Tuner**

       <img src="images/widget-view.png" alt="Figure 3" width="700"/>

    7. You can view the raw count, baseline, difference count and status for each sensor and also the Touchpad position in the **Graph View** tab. For example, to view the sensor data for Touchpad 0, select **Touchpad0_Col0** under **Touchpad0**.

       **Figure 4. Graph View of the CapSense Tuner**

       <img src="images/graph_view.png" alt="Figure 4" width="700"/>

    8. The **Touchpad View** tab shows the heatmap view and the finger movement can be visualised on the same.

       **Figure 5. Touchpad View of the CapSense Tuner**

       <img src="images/touchpad-view.png" alt="Figure 5" width="700"/>

    9. Observe the **Widget/Sensor Parameters** section in the CapSense Tuner window. The Compensation CDAC values for each touchpad sensor element calculated by the CapSense resource is displayed as shown in **Figure 15**. 
      
   10. Verify that the SNR is greater than 5:1 by following the steps given in [Stage 4](#stage-4-obtain-cross-over-point-and-noise) starting with Step 6.
   
     Non-reporting of false touches and the linearity of the position graph indicate a proper tuning.
   
   **Note:** The Touchpad performance can be further enhanced by configuring the board to connect the Hatch pattern to Shield. To do so, see the kit user guide for the board connection settings. Also ensure to enable the Shield Signals in the CapSense configurator as shown in **Figure 6**. Then retune the Touchpad following the Tuning process as given in the [Tuning Procedure](#tuning-procedure).

    **Figure 6. Enabling Shield Signals in CapSense Configurator**

    <img src="images/shield-settings.png" alt="Figure 6" width="700"/>

## Operation

The following steps explain the tuning procedure. 

**Note:** See the section "Selecting CapSense Hardware Parameters" in the [PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/AN85951) to learn about the considerations for selecting each parameter values.

### Tuning Procedure

**Figure 7. CSD Touchpad Widget Tuning Flow**  

<img src="images/flowchart_for_tuning.png" alt="Figure 7" width="500" />
   
Do the following to tune the touchpad: 

- [Stage 1. Measure the Parasitic Capacitance (Cp)](#stage-1-measure-the-parasitic-capacitance-cp)

- [Stage 2. Calculate the Sense Clock Frequency](#stage-2-calculate-the-sense-clock-frequency)

- [Stage 3. Set the Initial Hardware Parameters](#stage-3-set-the-initial-hardware-parameters)

- [Stage 4. Obtain Cross-over point and Noise](#stage-4-obtain-cross-over-point-and-noise)

- [Stage 5. Use the CapSense Tuner to Fine-Tune Sensitivity for 5:1 SNR](#stage-5-use-the-capsense-tuner-to-fine-tune-sensitivity-for-51-snr)

- [Stage 6. Use CapSense Tuner to Tune Threshold Parameters](#stage-6-use-capsense-tuner-to-tune-threshold-parameters)

#### Stage 1. Measure the Parasitic Capacitance (Cp)

To determine the maximum Row/Column Cp, measure the Cp of each sensor element of the touchpad, between the sensor electrode (sensor pin) and device ground, using an LCR meter or by back-calculating for Cs (Sensor capacitance) using the Raw Count equation. 

See the [PSoC 4: MSC CapSense CSD Button Tuning](https://github.com/cypresssemiconductorco/mtb-example-psoc4-msc-capsense-csd-button-tuning) code example "**Equation 1. CSD-RM Raw Count**" for the Raw Count equation.

An example of Cp calculation using the Raw Count(without placing the finger) is as shown below:

Rawcount = (Nsub/Cref). (Cs - 2.(SnsClkDiv/CompClkDiv).Ccomp)

Cs = (Rawcount * Cref)/Nsub + 2.(SnsClkDiv/CompClkDiv).Ccomp 

Cs = (4981 * 27 * 8.86)/60 + 2.(96/24).(146*8.86) = 30.2 pF

**Note 1:** Reference Capacitance (Cref) and Compensation Capacitance (Ccomp) is calculated by multiplying the Reference CDAC Value and Compensation CDAC Value with the Clsb (8.86 fF) respectively.

**Note 2:** In order to get the true value of the Parasitic Capacitance of a sensor, the inactive sensors need to be Grounded.

   **Table 1. Cp Values Obtained for CY8CKIT-041S-MAX Kit**

   | Kit     | Maximum Column Parasitic Capacitance (C<sub>P_Col</sub>) in pF | Maximum Row Parasitic Capacitance (C<sub>P_Row</sub>) in pF |
   |:---------------------|:----------------------------------------| :-----|
   | CY8CKIT-041S-MAX kit            |          40      | 40 |


#### Stage 2. Calculate the Sense Clock Frequency

1. Calculate the row and column sense clock frequencies using **Equation 1**.

   **Equation 1. Max Sense Clock Frequency**

   <img src="images/fsw-equation.png" alt="Equation 1" width="300" />

    Where,
   - F<sub>SW(Row)</sub> and  F<sub>SW(Col)</sub> are the Row and Column Sense Clock Frequency, respectively.

   - C<sub>P(Row)</sub> and  C<sub>P(Col)</sub> are the maximum parasitic capacitance of the Row/Column electrode respectively.

   - R<sub>Series</sub> is the maximum total series resistance, which includes the 500-ohm pin internal resistance, the external series resistance (in CY8CKIT-041S MAX, it is 2 kilo-ohm), and the trace resistance. Include the trace resistance if high-resistive material such as ITO or conductive ink is used. The external resistor is connected between the sensor pad and the device pin to reduce the radiated emission. ESD protection is built into the device.

   **Note:** If LCR meter is not available, set an initial sense clock divider value and look at the charge and discharge waveforms of the sensor electrode and iteratively change the divider and set a maximum frequency such that it completely charges and discharges in each phase of the MSC CSD sensing method.

2. Ensure that the following conditions are also satisfied when selecting the sense clock frequency and CDAC Compensation Divider:

   - The auto-calibrated CDAC and Compensation CDAC value should lie in the valid range (i.e., 6 - 200) for the selected sense clock divider and CDAC compensation divider. Verify this once the initial hardware parameters are loaded into the device. See **Step 3** (*Ensure that the auto-calibrated CDAC is within the recommended range*) of [Stage 4](#stage-4-obtain-cross-over-points-and-noise) for more details.

   - If you are explicitly using the PRS or SSCx clock source to lower electromagnetic interference, ensure that you select the sense clock frequency that meets the conditions mentioned in the [ModusToolbox CapSense Configurator Guide](https://www.cypress.com/file/492896/download) in addition to the above conditions. PRS and SSCx techniques spread the frequency across a range. The maximum frequency set should charge and discharge the sensor completely, which you can verify using an oscilloscope and an active probe.

   -  If you want to scan Channel 0 and Channel 1 in the same scan slot simultaneously, the Sense clock divider and the number of sub-conversion should be the same for both sensor elements.

     **Table 2. Sense Clock Frequency Settings for CY8CKIT-041S-MAX Kit**

    | Kit | R<sub>SeriesTotal</sub> (kΩ) | Maximum Column Cp (pF) | Maximum Row Cp (pF) | Maximum Column Sense Clock Frequency (kHz) | Maximum Row Sense Clock Frequency (kHz) |
    | :--------- | :------------    | :------------ | :------- |:----|:----| 
    | CY8CKIT-041S-MAX | 2500 | 40 | 40 | 500 | 500| 

   The calculated value, listed in Table 2, ensures the maximum possible sense clock frequency (for good gain and CDAC code lying in the valid range) while allowing the sensor capacitance to fully charge and discharge in each phase of the MSC CSD sensing method. In addition, also ensure that the shield waveform is probed and that it satisfies the condition given in the section **Shield Electrode Tuning Theory** in the [CapSense Design guide](https://www.cypress.com/AN85951).

#### Stage 3. Set the Initial Hardware Parameters

1. Connect the board to your PC using the provided USB cable through the KitProg3 USB connector.

2. Launch the Device Configurator tool.
   
   You can launch the Device Configurator in Eclipse IDE for ModusToolbox from the Tools section in the IDE Quick Panel or in stand-alone mode from *{ModusToolbox install directory}/ModusToolbox/tools_{version}/device-configurator/device-configurator*. In this case, after opening the application, select **File** > **Open** and open the *design.modus* file of the respective application, which is present in the *{Application root directory}/COMPONENT_CUSTOM_DESIGN_MODUS/TARGET_\<BSP-NAME>* folder.

3. In the [CY8CKIT-041S MAX kit](https://www.cypress.com/documentation/development-kitsboards/psoc-4100s-max-pioneer-kit-cy8ckit-041s-max), the touchpad pins are connected to both channel 0 and channel 1. Hence, make sure to enable channel 0 and channel 1 in the Device Configurator as shown in **Figure 8**.

     **Figure 8. Enable MSC Channels in Device Configurator**

     <img src="images/device-configurator-multi-channel.png" alt="Figure 8" width="900"/>

     Save the changes and close the window.

4. Launch the CapSense Configurator tool.
   
   You can launch the CapSense Configurator tool in Eclipse IDE for ModusToolbox from the 'CapSense' peripheral setting in the Device Configurator or directly from the Tools section in the IDE Quick Panel. You can also launch it in stand-alone mode from *{ModusToolbox install directory}/ModusToolbox/tools_{version}/capsense-configurator/capsense-configurator*. In this case, after opening the application, select **File** > **Open** and open the *design.cycapsense* file of the respective application, which is present in the *{Application root directory}/COMPONENT_CUSTOM_DESIGN_MODUS/TARGET_\<BSP-NAME>* folder.

   See the [ModusToolbox CapSense Configurator Tool Guide](https://www.cypress.com/ModusToolboxCapSenseConfig) for step-by-step instructions on how to configure and launch CapSense in ModusToolbox. 

5. In the **Basic** tab, note that a single touchpad **Touchpad0** is configured as a **CSD RM (Self-cap)** and the CSD tuning mode is set as **Manual tuning**. 

   **Figure 9. CapSense Configurator - Basic Tab**  

   <img src="images/capsense_basictab.png" alt="Figure 9" width="1050" />

6. Do the following in the **General** tab under the **Advanced** tab:
   - Set **Scan mode** as **CS-DMA** to enable autonomous scanning. 
   
     Automated scan using DMA is helpful for scanning multiple sensors autonomously and offloading the CPU. 

     Ensure to do the required DMA Settings in the **Device Configurator** as given in the section [DMA Connection Settings for CS-DMA mode](#dma-connection-settings-for-cs-dma-mode).

   - **Sensor connection method** is **CTRLMUX** by default for CS-DMA Scan mode. 
   
     CTRLMUX mode allows the MSC block to control the GPIO pins and removes the need of AMUXBUS to transfer CapSense signals between GPIO and the MSC block.

   - Set **Modulator clock divider** as **1** to obtain the maximum available modulator clock frequency as recommended in the [CapSense Design guide](https://www.cypress.com/AN85951).

       **Note:** The modulator clock frequency can be set to 48,000 kHz only after changing the IMO clock frequency to 48 MHz, since the Modulator clock is derived from the IMO clock. Do the following:

       1. Under the **System** tab in the **Device Configurator** tool, select **System Clocks** > **Input** > **IMO**. 
    
       2. Select **48** from the **Frequency(MHz)** drop-down list.
   
   - Retain the default settings for all filters. You can enable the filters later depending on the signal-to-noise ratio (SNR) requirements in [Stage 5](#stage-5-use-the-capsense-tuner-to-fine-tune-sensitivity-for-51-snr).
     
     Filters are used to reduce the peak-to-peak noise. Using filters will result in higher scan time.

   **Figure 10. CapSense Configurator - General Tab under the Advanced Tab**  

   <img src="images/adv_general.png" alt="Figure 10" width="700" />

7. Go to the **CSD Settings** tab and make the following changes:
  
   - Set **Inactive sensor connection** as **Shield**.

     Inactive sensors connected to Shield provides better performance in terms of SNR and Refresh rate (as the use of shield results in reduction of sensor Cp) and can also be used if your design requires liquid tolerance.  

   - Set **Shield mode** as **Active**.

     MSC provides active and passive shielding. Passive shielding is selected if the total Shield Cp is less than 100 pF, and helps to save power. Before enabling this option, ensure that the PCB has the shield electrode. 

   - Select **Enable CDAC auto-calibration** and **Enable compensation CDAC**. 
   
     This helps in achieving the required CDAC calibration levels (85% of maximum count) for all sensors in the widget while maintaining the same sensitivity across the sensor elements.

   **Figure 11. CapSense Configurator - CSD Settings Tab under the Advanced Tab**  

   <img src="images/adv_csd.png" alt="Figure 11" width="700" />

8. Go to the **Widget Details** tab. Select **Touchpad0** from the left pane, and then set the following:
   
   - **Maximum X-Axis position** and **Maximum Y-Axis position** to **160** and **100**, respectively, as it is a 16*10 touchpad.

   - **Column sense clock divider:** **96**

   - **Row sense clock divider:** **96**

      **Note:** The Sense clock divider value, as given by **Equation 2**, is obtained by dividing HFCLK (48 MHz) by **Maximum Column Sense Clock Frequency (kHz)** and **Maximum Row Sense Clock Frequency (kHz)** calculated in [Stage 2](#stage-2-calculate-the-sense-clock-frequency) (see **Table 2**) and choosing the nearest ceiling sense clock divider option in the Configurator.
      
      **Equation 2. Sense Clock Divider**

      ![Equation 2](images/scd-equation.png)

      In this case, sense clock divider = 48000/500 = 96.
   
   - **Clock source:** **Direct**

      **Note:** Spread Spectrum Clock (SSC) or PRS clock can be used as a clock source to deal with EMI/EMC issues. The selected value should be set using the **Widget Details** tab in the CapSense Configurator.

   - **Number of sub-conversions: 60**

     60 is a good starting point to ensure a fast scan time and sufficient signal. This value will be adjusted as required in [Stage 5](#stage-5-use-the-capsense-tuner-to-fine-tune-sensitivity-for-51-snr).

   - **Finger Threshold: 20**

     Finger Threshold is initially set to a low value, which allows the *Touchpad View* to track the finger movement during tuning.

   - **Noise Threshold: 10** 
   
     This reduces the influence of baseline on the sensor signal, which helps to get the true difference-count. Retain the default values for all other threshold parameters; these parameters are set in [Stage 6](#stage-6-use-capsense-tuner-to-tune-threshold-parameters).

   - **CDAC Compensation divider: 96**

     CDAC compensation divider is used to set the clock that controls the Ccomp capacitor. It must be less than or equal to the sense clock divider. The ratio of Sense clock divider to CDAC compensation divider must be an integer. Set an initial value equal to the sense clock divider (i.e., 96). This will be adjusted to bring the CDAC code into the valid range (6-200) in the step 4 of [Stage 4](#stage-4-obtain-cross-over-points-and-noise).

     **Note:** This parameter is only effective when the compensation CDAC is enabled.
     
     **Equation 3. CDAC Compensation Divider**
   
     <img src="images/cdac-comp-equation.png" alt="Equation 3" width="300" />

   **Figure 12. CapSense Configurator - Widget Details Tab under the Advanced Tab**  
      
   <img src="images/adv_widget_detail.png" alt="Figure 12" width="700" />

7. Go to the **Scan Configuration** tab to select the pins and the scan slots. Do the following:

   Set the parameters in the **Scan configuration** tab as shown in **Figure 13**.

   **Figure 13. Scan Configuration Tab**

   <img src="images/scan-configuration.png" alt="Figure 13" width="800"/>

   <img src="images/scan-configuration-col10-col15.png" alt="Figure 13" width="800"/>

   1. Configure channels for the Row and Column electrodes using the drop-down menu.

      As seen in **Figure 13**, the Row and Column pins are divided equally between channel 0 and channel 1.

   2. Configure pins for the electrodes using the drop-down menu.

   3. Configure the scan slots.

       The summary section in the **Scan configuration** tab shows 13 scan slots (for 26 sensors), with each channel scanning in each slot simultaneously. This helps to perform parallel scanning and reduces the total scan time. 

   - Each sensor is allotted a scan slot based on the entered slot number.

   - The slots are assigned such that the sensors which are under parallel scanning are as far as possible from each other.

      **Note:** Row sensors cannot share a scan slot with a column sensor. In a single scan slot, either two rows or two columns can be scanned, and not the combination of them.

     See [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/an85951) for more details on Scan slot allotment rules. 

   - Check the notice list for warning or errors.

8. Click **Save** to apply the settings.

#### Stage 4. Obtain Cross-over point and Noise 

1. Program the board.

2. Launch the CapSense Tuner to monitor the CapSense data and for CapSense parameter tuning and SNR measurement.
 
   See the [CapSense Tuner Guide](https://www.cypress.com/ModusToolboxCapSenseTuner) for step-by-step instructions on how to launch and configure the CapSense Tuner in ModusToolbox.

3. Ensure that the auto-calibrated CDAC is within the recommended range.

   As discussed in step 2 of [Stage 2](#stage-2-calculate-the-sense-clock-frequency), the sense clock divider or CDAC compensation divider will be tuned to bring the CDAC values to the recommended range in this step. 
   
   - Click **Touchpad0** in the **Widget Explorer** to view the Column reference CDAC value and Row reference CDAC value in the sensor parameters window as shown in **Figure 15**.
   - Also, click each sensor element, for instance, **Touchpad0_Col0** in the **Widget Explorer** to view the Compensation CDAC in the sensor parameters window as shown in **Figure 15**. 
   - If the CDAC values are within the range 6 to 200, the following step is not required.
 
4. Fine-tune the sense clock divider or CDAC compensation divider to bring the CDAC value within range.

   From the Raw Count equation (given in the [PSoC 4: MSC CapSense CSD Button Tuning](https://github.com/cypresssemiconductorco/mtb-example-psoc4-msc-capsense-csd-button-tuning) Code Example), it is evident that increasing the sense clock divider will decrease the reference CDAC value for a given calibration percent and vice versa. Increasing the CDAC compensation divider will increase the compensation CDAC for a given calibration percent and vice versa.

   1. If the Column reference CDAC value and Row reference CDAC value are not in the recommended range, increase the Column sense clock divider and Row sense clock divider, respectively, in the Widget Hardware Parameters window.

   2. Click **To Device** to apply the changes to the device as shown in **Figure 14**.
      
   3. Click each sensor element, for instance, **Touchpad0_Col0** in the Widget Explorer.

   4. Observe the Compensation CDAC value in the **Sensing Parameters** section of the Widget/Sensor Parameters window.

   5. Decrease the CDAC compensation divider such that the ratio of Sense clock divider and the CDAC compensation divider is an integer, in the Widget Hardware Parameters window.
   
   6. Click **To Device** to apply the changes to the device as shown in **Figure 14**.

      **Figure 14. Apply Changes to Device**

      <img src="images/tuner-apply-to-device.png" alt="Figure 14" width="600"/>

   7. Repeat steps 1 to 6 until you obtain all CDAC values in the range of 6 to 200.

      **Figure 15. CDAC Value**

      <img src="images/tuner-cdac-setting.png" alt="Figure 15" width="500"/>

      **Note:** As **Figure 15** shows, CDAC values are in the recommended range when the CDAC Compensation divider is decreased to 24. You can leave the sense clock divider to the value as shown in Step 8 of [Stage 3](#stage-3-set-the-initial-hardware-parameters).

4. Capture the raw counts of each sensor element in the touchpad (as shown in **Figure 16**) and verify that they are approximately (+/- 5%) equal to 85% of the MaxCount. See **Equation 1** in [PSoC 4: MSC CapSense CSD Button Tuning](https://github.com/cypresssemiconductorco/mtb-example-psoc4-msc-capsense-csd-button-tuning) Code Example for the MaxCount equation.

   1. Go to the **Touchpad View** tab and change the **Display settings** as follows:

      - **Display mode:** - Touch Reporting

      - **Data type:** - **RawCount**

      - **Value type:** - **Current**

      - **Number of samples:** - **1000**

    **Figure 16. Rawcounts obtained on the Touchpad View Tab in Tuner Window**  

     <img src="images/touchpad-view-rc.png" alt="Figure 16"  width="1050" />
 
5. Capture and note the peak-to-peak noise of each sensor element in the touchpad. 

   1. From the **Widget Explorer** section, select the widget *Touchpad0*. 

   2. Go to the **Touchpad View** tab and change the **Display settings** as follows:

      - **Display mode:** - Touch Reporting

      - **Data type:** - **RawCount**

      - **Value type:** - **Max-Min**

      - **Number of samples:** - **1000**
   
      Capture the variation in the rawcounts for 1000 samples, without placing a finger (which gives the peak-to-peak noise) and note the highest noise.

    **Figure 17. Noise obtained on the Touchpad View Tab in Tuner Window**  

     <img src="images/touchpad-view-noise.png" alt="Figure 17"  width="1050" />

     **Table 3. Max Peak-to-Peak Noise Obtained in CY8CKIT-041S-MAX**
   
      |Kit | Max Peak-to-peak Noise for Row Sensors| Max Peak-to-Peak Noise for Column Sensors|
      |:----------|:-------------------------| :------|
      |CY8CKIT-041S-MAX   | 7 | 5|

6. View the Row and Column sensor signals in the **Graph View** tab in the **Sensor Signal** graph display by swiping your finger across the touchpad from top to bottom and left to right respectively.

      **Known issue:** Final version of Auto-calibration algorithm is under development, and hence the existing Auto-calibration algorithm may not result in equal sensitivity among all sensors.

      **Work-around:**

      View the Row Signals and Column Signals separately in the **Graph View** tab to ensure that they have similar sensitivity as shown in **Figure 18**.

      **Figure 18. Row Signals obtained on the Graph View Tab in Tuner Window**  

      <img src="images/graph-view-signals.png" alt="Figure 18"  width="570" />

      If not, save the tuner settings by clicking **Apply to device** and **Apply to Project** to save the Configuration parameters obtained through auto-calibration. Close the Tuner. Open the CapSense configurator and disable Auto-Calibration. Reprogram the device.

      Now tweak the Compensation CDAC codes of each sensor element to obtain equal sensitivity as in **Figure 18**.

      **Note:** Make sure that the rawcount lies within the range +/- 5% of 85% of the Maximum rawcount.

7. Firmly hold the finger (typically 8 mm or 9 mm) on the touchpad in the Least Touch Intensity (LTI) position (at the intersection of 4 nodes) as shown below. See [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/an85951) for more details on Least Touch Intensity (LTI) position. 
   
   **Figure 19. Least Touch Intensity (LTI) position**  

   <img src="images/lti-position.png" alt="Figure 19" width="100" />

   **Note:** Finger movement during the test can artificially increase noise level.

      1. Go to the **Touchpad View** tab and change the **Display settings** as follows:
          
         - **Display mode:** - Touch Reporting

         - **Data type:** - **DiffCount**

         - **Value type:** - **Current**

         Place the finger such that equal signal is obtained in the two intersecting column and row sensors respectively (look at the heatmap displayed in the **Touchpad View** tab as shown in **Figure 20**).

         **Note:** The LTI signal is measured at the farthest point (not at the last column/row) of the touchpad from the sensor pin connection, where the sensors have the worst case RC-time constant.

      **Figure 20. LTI Position in Touchpad View**  

      <img src="images/touchpad-view-lti.png" alt="Figure 20" width="1200" />

      Row LTI Signal = (88 + 87 )/2 = 87, Column LTI Signal = (52 + 52)/2 = 52

#### Stage 5. Use the CapSense Tuner to Fine-Tune Sensitivity for 5:1 SNR

The CapSense system may be required to work reliably in adverse conditions such as a noisy environment. The touchpad sensors need to be tuned with SNR > 5:1 to avoid triggering false touches and to make sure that all intended touches are registered in these adverse conditions.

**Note:** For gesture detection, it is recommended to have around 10:1 SNR.

1. Ensure that the LTI Signal count is greater than 50 and meets at least 5:1 SNR (using **Equation 4**). 

   In the CapSense Tuner window, increase the **Number of sub-conversions** (located in the **Widget/Sensor Parameters** section, under **Widget Hardware Parameters**) by 10 until you achieve atleast 5:1 SNR.

   **Equation 4:** 

   <img src="images/snr-equation.png" alt="Equation 4" width="200" />

   Where,
    
    - LTI Signal is the Signal obtained as in **Figure 20**
    
    - Pk-Pk noise is the peak-to-peak noise obtained as in **Figure 17**

    SNR is measured for Row sensors and Column sensors seperately, using **Equation 4**.
   
   Here, from **Figure 17** and **Figure 20**, 
   
   SNR of Column Sensors = 52/5 = 10.4, SNR of Row Sensors = 87/7 = 12.4

   **Note:** Ensure that the **Number of Sub conversions** (Nsub) do not exceed the maximum limit and saturate the raw count.

   Use **Equation 5** to calculate maximum number of sub conversions: 

   **Equation 5:** 
   
   ![Equation 5](images/nsub-eqn.png)

   Max Nsub = 2^16/96 = 65536/96 = 682 [16 bit counter]

   Nsub is also tuned to satisfy the Refresh rate that is required. 

   **Equation 6. Scan Time**

   <img src="images/scan-time-equation.png" alt="Equation 6" width="150"/>

   **Note:** Total scan time is equal to the sum of Initialization time and the scan time given by **Equation 6**.

2. After changing the **Number of sub-conversions**, click **Apply to Device** to send the setting to the device. The change is reflected in the graphs.

   **Note:** The **Apply to Device** option is enabled only when the **Number of sub-conversions** is changed.

####  Verify Refresh Rate and Response time

   1. Response time of the touchpad can be visualised using LEDs to indicate toggling of sensor GPIOs when touched. 

   2. Refresh rate is a combination of sensor initialization time, scan time (as per **Equation 6**), processing time, and tuner communication time, which can be verified using the tuner as shown in **Figure 21**.

      **Figure 21. Refresh Rate Measurement** 

      <img src="images/tuner-refresh-rate.png" alt="Figure 21" width="700"/>

   3. You can also measure the refresh rate by toggling one of the GPIOs in each sensor scan loop. Probing the GPIO (P10.4 on J3) on the Oscilloscope shows the refresh rate as shown in **Figure 22**.

      **Figure 22. Probing GPIO for Refresh Rate** 

      <img src="images/probed-refresh-rate.png" alt="Figure 22" width="400"/>

      **Note:** Refresh rate obtained here is with the tuner closed, as Tuner is only used for the purpose of debugging and will not play a role in deciding the Refresh rate in the end application. 
      
      Refresh Rate from **Figure 22** = 1 /2.2 ms = 454 Hz

#### Stage 6. Use CapSense Tuner to Tune Threshold Parameters

After confirming that your design meets the timing parameters, and the SNR is greater than 5:1, set your threshold parameters.

**Note:** Thresholds are set based on LTI Position, because it is the least valid touch signal that can be obtained.

  1. Set the recommended threshold values for the Touchpad widget using the LTI Signal value obtained in [Stage 5](#stage-5-use-the-capsense-tuner-to-fine-tune-sensitivity-for-51-snr):
   - **Finger Threshold** – 80% of the lower LTI Signal (either Row or Column)
   - **Noise Threshold** – Twice the highest noise or 40% of the lower LTI signal (whichever is greater)
   - **Negative Noise Threshold** – Twice the highest noise or 40% of the lower LTI (whichever is greater)
   - **Hysteresis** - 10% of the lower LTI signal
   - **ON Debounce** – Default value of 3 (Set to 1 for gesture detection)
   - **Low Baseline Reset** - Default value of 30
   - **Velocity** - Default value of 2500
   
     **Note:** Velocity parameter is not required for single finger detection.

   **Table 4. Software Tuning Parameters obtained for CY8CKIT-041S-MAX** 
  
   |Parameter|	 CY8CKIT-041S-MAX|
   |:--------|:------|
   |Number of Sub-conversions	| 60	| 
   |Finger Threshold 	| 41| 
   |Noise Threshold |20| 
   |Hysteresis	| 5 | 
   |ON Debounce	|3| 
   |Low Baseline Reset	| 30| 
   |Negative Noise Threshold	|20 | 

### Applying Settings to Firmware

1. Click **Apply to Device** and **Apply to Project** in the CapSense Tuner window to apply the settings to the device and project, respectively. Close the tuner.

   **Figure 23. Apply to Project**

   <img src="images/apply_to_project.png" alt="Figure 23" width="500" />

   The change is updated in the *design.cycapsense* file and reflected in the CapSense Configurator.

## Debugging

You can debug the example to step through the code. In the IDE, use the **\<Application Name> Debug (KitProg3_MiniProg4)** configuration in the **Quick Panel**. For more details, see the "Program and Debug" section in the [Eclipse IDE for ModusToolbox User Guide](https://www.cypress.com/MTBEclipseIDEUserGuide).

## Design and Implementation

The project uses the [CapSense middleware](https://cypresssemiconductorco.github.io/capsense/capsense_api_reference_manual/html/index.html); see the [ModusToolbox User Guide](http://www.cypress.com/ModusToolboxUserGuide) for more details on selecting a middleware. 

See [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/an85951) for more details of CapSense features and usage. 

The design has a Ratiometric Self-capacitance (CSD-RM) based, 26-element (10+16) CapSense Touchpad and EZI2C peripheral. The EZI2C Slave peripheral is used to monitor the sensor data and touchpad touch position information on a PC using the CapSense tuner available in the Eclipse IDE for ModusToolbox via I2C communication.  

The code scans a touchpad widget using the CSD-RM sensing method and sends the CapSense raw data over an I2C interface to the CapSense Tuner GUI tool on a PC using the onboard KitProg USB-I2C bridge.

#### **Steps to Setup the VDDA Supply Voltage in Device Configurator**

1. Open Device Configurator from the Quick Panel. 

2. Go to the **System** tab. Select the **Power** resource, and set the set the VDDA value under **Operating Conditions** as shown in **Figure 24**. 

   **Figure 24. Setting the VDDA Supply in System tab of Device Configurator**

   <img src="images/vdda-settings.png" alt="Figure 24" width="600"/> 

**Note**: [PSoC 4100S Max Pioneer Kit](https://www.cypress.com/documentation/development-kitsboards/psoc-4100s-max-pioneer-kit-cy8ckit-041s-max) has two onboard regulators 3.3V and 5V. To use 3.3V, place the jumper J10 at positions 2 and 3. See the kit user guide for more details.

#### **DMA Connection Settings for CS-DMA mode**

1. Make sure that the Scan mode parameter in the CapSense Configurator is set to **CS-DMA** (as shown in **Figure 10**).

2. Launch the Device Configurator and select the MSC instance for which the DMA configuration needs to be set. The figure below illustrates the selection for the case where DMA is configured for MSC0.

   Switch to the Parameters panel and select the connection for each DMA trigger. Each trigger must be connected to a separate DMA channel as shown in **Figure 25**.

   - Connect the Write DMAC Channel Trigger Output to the DMAC Channel 0.
   - Connect the Chain Write DMAC Channel Trigger Input to the DMAC Channel 1.
   - Connect the Chain Read DMAC Channel Trigger Input to the DMAC Channel 2.
   - Connect the Read DMAC Channel Trigger Output to the DMAC Channel 3.

   **Figure 25. DMA Configuration on Peripherals tab**

   <img src="images/dma-configuration-on-peripherals-tab.png" alt="Figure 25" width="600"/> 

3. Similarly, select the connection for each DMA trigger for MSC1 (If both the channels are enabled).The triggers must be connected to the DMA channels in the following way:

   - Connect the Write DMAC Channel Trigger Output to the DMAC Channel 4.
   - Connect the Chain Write DMAC Channel Trigger Input to the DMAC Channel 5.
   - Connect the Chain Read DMAC Channel Trigger Input to the DMAC Channel 6.
   - Connect the Read DMAC Channel Trigger Output to the DMAC Channel 7.

4. Switch to the DMA tab. Connect the tr_out of the DMAC Channel that has been selected for the Chain Write DMAC Channel Trigger Input signal, with the tr_in of the DMAC Channel that has been selected for the Write DMAC Channel Trigget Output signal as shown in the figure below:

   **Figure 26. Write Channel DMA Configuration on DMA tab**

   <img src="images/write-dmac-channel-settings-dma-tab.png" alt="Figure 26" width="600"/>

5. Connect the tr_out of the DMAC Channel that has been selected for the Read DMAC Channel Trigger Input signal, with the tr_in of the DMAC Channel that has been selected for the Chain Read DMAC Channel Trigger Output signal as shown in the figure below:

   **Figure 27. Read Channel DMA Configuration on DMA tab**

   <img src="images/read-dmac-channel-settings-dma-tab.png" alt="Figure 27" width="600"/>

6. Repeat the steps 4 and 5 for MSC1 channel (if/when required).

7. Perform the **File**  > **Save** command and check the **Notice List**. It should be empty.

8. Fix the issues in case of any messages in the list.

### Resources and Settings

See the [Operation](#operation) section for step-by-step instructions to configure the CapSense Configurator.

**Figure 28. Device Configurator - EZI2C Peripheral Parameters**

 <img src="images/ezi2c_config.png" alt="Figure 28" width="500"/>

The following ModusToolbox resources are used in this example:

**Table 5. Application Resources**

| Resource  |  Alias/Object     |    Purpose     |
| :------- | :------------    | :------------ |
| SCB (I2C) (PDL) | CYBSP_EZI2C          | EZI2C slave Driver to communicate with CapSense Tuner GUI |
| CapSense | CYBSP_MSC0,CYBSP_MSC1 | CapSense driver to interact with the MSC hardware and interface the CapSense sensors |
| Digital Pin | CYBSP_USER_LED | To visualise the touchpad response |

### Firmware Flow

**Figure 29. Firmware Flowchart**

<img src="images/firmware_flow.png" alt="Figure 29" width="200"/>

## Related Resources

| Application Notes                                            |                                                              |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [AN79953](https://www.cypress.com/AN79953) – Getting Started with PSoC 4 | Describes PSoC 4 devices and how to build your first application with PSoC Creator |
| [AN85951](https://www.cypress.com/AN85951) – PSoC 4 and PSoC 6 MCU CapSense Design Guide  | Describes how to design capacitive touch sensing applications with the PSoC 6 families of devices |
| **Code Examples** | 
| [Using ModusToolbox](https://github.comcypresssemiconductorco/Code-Examples-for-ModusToolbox-Software) | [Using PSoC Creator](https://www.cypress.com/documentation/code-examples/psoc-345-code-examples) |
| **Device Documentation**                                
| [PSoC 4 MCU Datasheets](https://www.cypress.com/search/all/PSOC%204%20datasheets?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) | [PSoC 4 Technical Reference Manuals](https://www.cypress.com/search/all/PSoC%204%20Technical%20Reference%20Manual?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) |
| **Development Kits**
| Buy at www.cypress.com                                     |
| [CY8CKIT-041S-MAX](https://www.cypress.com/CY8CKIT-041S-MAX) PSoC 4100S Max Pioneer Kit |
**Libraries**                                                 |                                                              |
| PSoC 4 Peripheral Driver Library (PDL) and docs  | [mtb-psoc4-pdl](https://github.com/cypresssemiconductorco/mtb-pdl-cat2) on GitHub |
| Cypress Hardware Abstraction Layer (HAL) Library and docs     | [mtb-psoc4-hal](https://github.com/cypresssemiconductorco/mtb-hal-cat2) on GitHub |
| **Middleware**                                               |                                                              |
| CapSense® library and docs                                    | [capsense](https://github.com/cypresssemiconductorco/capsense) on GitHub |
| **Tools**                                                    |                                                              |
| [Eclipse IDE for ModusToolbox](https://www.cypress.com/modustoolbox)     | The cross-platform, Eclipse-based IDE for IoT designers that supports application configuration and development targeting converged MCU and wireless systems.             |
| [PSoC Creator™](https://www.cypress.com/products/psoc-creator-integrated-design-environment-ide) | The Cypress IDE for PSoC and FM0+ MCU development.            |

## Other Resources

Cypress provides a wealth of data at www.cypress.com to help you select the right device, and quickly and effectively integrate it into your design.


## Document History

Document Title: *CE232273* - *PSoC 4: MSC Self-Capacitance Touchpad Tuning*

| Version | Description of Change |
| ------- | --------------------- |
| 1.0.0   | New code example      |

------

All other trademarks or registered trademarks referenced herein are the property of their respective owners.

![banner](images/ifx-cy-banner.png)

-------------------------------------------------------------------------------

© Cypress Semiconductor Corporation (An Infineon Technologies Company), 2021. This document is the property of Cypress Semiconductor Corporation, and Infineon Technologies company, and its affiliates (“Cypress”). This document, including any software or firmware included or referenced in this document (“Software”), is owned by Cypress under the
intellectual property laws and treaties of the United States and other countries worldwide. Cypress reserves all
rights under such laws and treaties and does not, except as specifically stated in this paragraph, grant any
license under its patents, copyrights, trademarks, or other intellectual property rights. If the Software is not
accompanied by a license agreement and you do not otherwise have a written agreement with Cypress
governing the use of the Software, then Cypress hereby grants you a personal, non-exclusive, nontransferable
license (without the right to sublicense) (1) under its copyright rights in the Software (a) for Software provided in
source code form, to modify and reproduce the Software solely for use with Cypress hardware products, only
internally within your organization, and (b) to distribute the Software in binary code form externally to end users
(either directly or indirectly through resellers and distributors), solely for use on Cypress hardware product units,
and (2) under those claims of Cypress’s patents that are infringed by the Software (as provided by Cypress,
unmodified) to make, use, distribute, and import the Software solely for use with Cypress hardware products.
Any other use, reproduction, modification, translation, or compilation of the Software is prohibited.
TO THE EXTENT PERMITTED BY APPLICABLE LAW, CYPRESS MAKES NO WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, WITH REGARD TO THIS DOCUMENT OR ANY SOFTWARE OR ACCOMPANYING
HARDWARE, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY
AND FITNESS FOR A PARTICULAR PURPOSE. No computing device can be absolutely secure. Therefore,
despite security measures implemented in Cypress hardware or software products, Cypress shall have no
liability arising out of any security breach, such as unauthorized access to or use of a Cypress product.
CYPRESS DOES NOT REPRESENT, WARRANT, OR GUARANTEE THAT CYPRESS PRODUCTS, OR
SYSTEMS CREATED USING CYPRESS PRODUCTS, WILL BE FREE FROM CORRUPTION, ATTACK,
VIRUSES, INTERFERENCE, HACKING, DATA LOSS OR THEFT, OR OTHER SECURITY INTRUSION
(collectively, “Security Breach”). Cypress disclaims any liability relating to any Security Breach, and you shall
and hereby do release Cypress from any claim, damage, or other liability arising from any Security Breach. In
addition, the products described in these materials may contain design defects or errors known as errata which
may cause the product to deviate from published specifications. To the extent permitted by applicable law,
Cypress reserves the right to make changes to this document without further notice. Cypress does not assume
any liability arising out of the application or use of any product or circuit described in this document. Any
information provided in this document, including any sample design information or programming code, is
provided only for reference purposes. It is the responsibility of the user of this document to properly design,
program, and test the functionality and safety of any application made of this information and any resulting
product. “High-Risk Device” means any device or system whose failure could cause personal injury, death, or
property damage. Examples of High-Risk Devices are weapons, nuclear installations, surgical implants, and
other medical devices. “Critical Component” means any component of a High-Risk Device whose failure to
perform can be reasonably expected to cause, directly or indirectly, the failure of the High-Risk Device, or to
affect its safety or effectiveness. Cypress is not liable, in whole or in part, and you shall and hereby do release
Cypress from any claim, damage, or other liability arising from any use of a Cypress product as a Critical
Component in a High-Risk Device. You shall indemnify and hold Cypress, including its affiliates, and its
directors, officers, employees, agents, distributors, and assigns harmless from and against all claims, costs,
damages, and expenses, arising out of any claim, including claims for product liability, personal injury or death,
or property damage arising from any use of a Cypress product as a Critical Component in a High-Risk Device.
Cypress products are not intended or authorized for use as a Critical Component in any High-Risk Device
except to the limited extent that (i) Cypress’s published data sheet for the product explicitly states Cypress has
qualified the product for use in a specific High-Risk Device, or (ii) Cypress has given you advance written
authorization to use the product as a Critical Component in the specific High-Risk Device and you have signed
a separate indemnification agreement.
Cypress, the Cypress logo, and combinations thereof, WICED, ModusToolBox, PSoC, CapSense, EZ-USB, FRAM, and Traveo are trademarks or registered trademarks of Cypress or a subsidiary of Cypress in the United
States or in other countries. For a more complete list of Cypress trademarks, visit cypress.com. Other names
and brands may be claimed as property of their respective owners.
