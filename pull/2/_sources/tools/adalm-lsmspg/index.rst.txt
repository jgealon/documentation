ADALM-LSMSPG
============

Active Learning Module for Low‑Speed Mixed‑Signal Exploration and Prototyping.

Overview
--------

.. figure:: adalm-lsmspg_angle.jpeg
   :align: left

   ADALM-LSMSPG

The :adi:`ADALM-LSMSPG (Analog Devices Active Learning Module - Low-Speed Mixed-Signal Playground)` facilitates hands-on learning of concepts related to software interfacing to hardware peripherals.

It consists of two 8-channel multifunction ADC/DAC/digital GPIOs -  AD5592r and AD5593r - which have SPI and I2C interfaces, respectively.

The AD5592r includes a simple NPN transistor curve tracer circuit as a representative instrumentation application.

The AD5593r includes a PNP curve tracer, and both devices include bi-color LEDs for "Hello, World!" applications.

An LM75 temperature sensor is also included as a "bare minimum" peripheral example.

The ADALM-LSMSPG is compatible with Raspberry Pi and Feather form factor hosts, and an ADALM2000 interface allows monitoring of the SPI and I2C busses for driver development and debug.

Features
++++++++

- Compatible with Raspberry Pi platforms
- Compatible with Feather platforms
   - MAX32666FTHR
   - MAX32655FTHR
   - Other Feather platforms supporting 3.3V I/O
- ADALM2000 interface for digital debug
- AD5592r SPI 8-channel ADC/DAC/GPIO
- AD5593r I2C 8-channel ADC/DAC/GPIO
- NPN and PNP transistor curve tracer example application circuits
- LM75 temperature sensor
- SPI and I2C Pmod expansion headers

Applications
++++++++++++

- Demonstrating peripheral interfacing and software software for:
   - Embedded Linux
   - no-OS (Bare Metal)
   - Zephyr
   - Arduino

System Architecture
+++++++++++++++++++

.. figure:: ADALM-LSMSPG_block_diagram.png

   ADALM-LSMSPG Simplified Block Diagram

.. csv-table:: Specifications
   :file: specifications.csv
   :widths: 20, 10, 10, 10, 10, 40
   :header-rows: 1

What's Inside the Box?
++++++++++++++++++++++

Upon purchase of the ADALM-LSMSPG kit, the package comes with the following boards and accessories:

.. figure:: ad-bmse2e3w-sl_package_contents.png

   ADALM-LSMSPG Kit Contents

Components and Connections
++++++++++++++++++++++++++

.. figure:: revised_ad-bmse2e3w-sl_board_with_pin_labels.png

   Hardware Components and Connections

.. csv-table:: Hardware Part Functions
   :file: components.csv
   :widths: 5, 25, 60
   :header-rows: 1

System Evaluation
+++++++++++++++++

Follow the setup shown in the diagram below to get the board up and running. Ensure that the hardware parts and equipment are complete based on list of Equipment Needed. The banana plug cables used in this setup only have a maximum rating of 10A. Cables suitable for higher current rating must be used if the intended application operates at range higher than 10A. Note that the AD-BMSE2E3W-SL board can only accommodate up to 100A.

.. figure:: system_evaluation_set-up.png

   Evaluation Setup

Equipment Needed
^^^^^^^^^^^^^^^^

- 1x ADALM-MMSC Board (as DUT)
- Windows, Mac, or Linux host computer with available USB port
- Signal generator, :adi:`ADALM2000 <https://analogdevicesinc.github.io/documentation/tools/m2k/index.html>` or similar.
   - Generators in the audio range work great
   - PC sound card with Audacity is another option

.. warning::

   This reference design has not undergone compliant testing for EMI/ EMC standards for automotive. It is up for the user to do its qualification as the requirements vary depending on its end application or use cases.


.. esd-warning::

.. note::

   For high current applications requiring greater than 50 A, it is advisable to install a heat sink to protect the pre-charge, charge, and discharge MOSFETs from overheating.


   - The AD-BMSE2E3W-SL Kit has 7 available HEATSINK PIN-FIN W/TAPE (375424B00034G) easy-to-install, adhesive type, aluminum top mount heat sink than can be installed directly on top of the board.

   - Peel off the protective film from the bottom of each heat sink and firmly press each one on top of the following FETs:

   (1) Attach the 5 heat sinks on the top layer of the board (Q4, Q5, Q6, Q7, and Q9),
   (2) the remaining 2 heat sinks on the bottom layer of the board (Q3 and Q8).

Preparing the MCU
+++++++++++++++++

By default (upon purchase), the AD+BMSE2E3W+SL board comes with a MAX32625PICO programmer adapter that is loaded with firmware image.

Otherwise, if you are using a new MAX32625PICO programmer (that is not part of the original kit), make sure to flash it first with the correct firmware image before connecting it to the AD-BMSE2E3W-SL board. If you do not know how to load the image, follow the instructions below:

#. Download the firmware image:
   :downgit-max32625pico-firmware-images:`MAX32625PICO Firmware Image for MAX32690 <master:bin/max32625_max32690evkit_if_crc_swd_v1.0.7.bin>`

#. Do not connect the MAX32625PICO to the AD-BMSE2E3W-SL Board yet.

#. Connect the MAX32625PICO to the Host PC using the micro USB to USB cable.

#. Press the button on the MAX32625PICO. (Do not release the button until the MAINTENANCE drive is mounted)

#. Release the button once the MAINTENANCE drive is mounted.

#. Drag and drop (to the MAINTENANCE drive) the firmware image.

#. After a few seconds, the MAINTENANCE drive will disappear and be replaced by a drive named DAPLINK. This indicates that the process is complete, and the MAX32625PICO can now be used to flash the firmware of the AD-BMSE2E3W-SL Board.

Hardware Setup
++++++++++++++
The board utilizes the DC2472A battery emulator as input for cell voltage measurement. The DC2472A allows a cell voltage of 1.4 V (min) to 4.2 V (max). Follow below steps to set up the board for cell measurement:

#. Screw the two cell connector blocks to the two DC2472A battery emulators. Note that the first two terminals and the last terminal of each DC2472A cell connector must be left hanging (refer to below figure). Make sure to also set the last two terminals' input to low voltage or equivalent range of roughly 1.4V per cell.

   .. figure:: battery_emulator_pins.png

      DC2472A Battery Emulator Pins

#. Connect the DC2472A battery emulators to the ADBMSE2E3W-SL board through the cell connector blocks. Then, connect a micro-USB Type B cable to each DC2472A battery emulator and power the boards by connecting the other end of the cables to the Host PC.

   .. figure:: usb_emulator.png

      Connecting the DC2472A Battery Emulator

#. Set the DC2472A battery emulators to the lowest voltage by fully turning the Cell Voltage Adjustment Potentiometer counterclockwise.

#. Attach the MAX32625PICO programmer to the AD-BMSE2E3W-SL board using the 10-pin ribbon SWD cable. Power the MAX32625PICO using a micro-USB to USB cable connected to the Host PC.

   .. figure:: max32625_power_usb_pc.png

      Connecting the MAX32625PICO Programmer

#. Connect the alligator clip cable (red) to the VBATTP Pin or the 3rd of Pin 17 header of the DC2472A battery emulator. Then, insert the other end of the cable (banana jack plug) to TP16 (VBAT+ terminal) of the AD-BMSE2E3W-SL board.

   .. figure:: connector_supply_vbattp.png

      DC2472A's VBATT+ Supply Connector to AD-BMSE2E3W-SL's VBAT+ Terminal

#. Connect the alligator clip cable (black) to the GND (VBAT-) supply of the DC2472A battery emulator. Then, connect the other end of the cable to the Rsense (top side) of the AD-BMSE2E3W-SL.

   .. figure:: gnd_vbat-_to_gnd_sense.png

      DC2472A's VBAT- Supply Connector to AD-BMSE2E3W-SL's Rsense


#. Set the DC2472A battery emulators to HIGH voltage or equivalent to 4.1 V per cell by turning the Cell Voltage Adjustment Potentiometer clockwise.

#. Check the supply for the following test points as described in the diagram and table below. Make sure that the voltage levels are within the specified range.

   .. figure:: quick_test_points.png

      AD-BMSE2E3W-SL Test Points

   .. csv-table:: Hardware Supply Quick Test Point
     :file: test-points.csv
     :widths: 15, 30, 35, 20
     :header-rows: 1

#. Once all steps are completed, you are now ready to use this reference design and run the accompanying software found in the link below:

   .. note::

      The AD-BMSE2E3W-SL comes complete with firmware examples and easy-to-use application GUI. Access the software resources and see the setup procedure in the AD-BMSE2E3W-SL Software User Guide.

Resources
++++++++++++

- :adi:`ADBMS6830`
- :adi:`ADBMS2950`
- :adi:`ADBMS6822`
- :adi:`MAX32690`
- :adi:`LLT8303`
- :adi:`ADUM225N`
- :adi:`LTC7001`
- :adi:`LTC3639`

Design & Integration Files
++++++++++++++++++++++++++

.. admonition:: Download

   `AD-BMSE2E3W-SL Design Support Package <ad-bmse2e3w-sl-designsupport.zip>`_

   * Schematic
   * PCB Layout
   * Bill of Materials
   * Allegro Project

Guides&Sample Software
----------------------



Help and Support
----------------

For questions and more information, please visit the :ez:`EngineerZone Support Community </>`.

