ADALM-MMSC
==========

Mixed-Mode Signal Chain Active Learning Module.

Overview
--------

.. figure:: adalm-mmsc_angle.jpg
   :align: left

   ADALM-MMSC

The :adi:`ADALM-MMSC (Analog Devices Active Learning Module - Mixed-Mode Signal Chain)` is an educational board that facilitates hands-on exploration of fundamental signal chain concepts.

The core of the circuit is the AD4080, 40-Msps oversampling SAR analog to digital converter (ADC) with configurable digital filter and internal data capture memory (single-port FIFO).

The board includes a highly configurable 2nd order analog filter with adjustable cutoff frequencies, input attenuation, and filter gain.

An onboard selectable clock source provides sample rates from 5 Msps to 40 Msps. A MAX32666FTHR microcontroller runs an IIO server over a serial backend, allowing the use of standard tools such as Scopy, IIO Oscilloscope, Pyadi-iio, and ADI's MATLAB precision toolbox.

The Libiio library provides language bindings for C, C#, MATLAB, Python, and other languages.

Concepts demonstrated include:

- Digital filtering
- Analog vs. digital anti alias filtering
- Properties of different digital filters
- Noise Analysis
- Reference noise

Features
++++++++

- Multiple input connections
   - 100-mil posts compatible with the ADALM2000
   - SMA connections for benchtop signal generators
   - 1/8-inch phono jack for connection to sound cards and other audio sources
- Selectable input attenuator
   - 3:5 attenuation allows +/-5 V input swing
   - 3:50, 3:500 allows testing with low-amplitude signals for dynamic range measurements
- Configurable Sallen-Key filter with low-noise amplifier
   - Cutoff frequencies between XX kHz and YY kHz
   - Gains of unity, 2, and 5 for exploring noise optimization
- Onboard fully-differential amplifier
   - Allows arbitrary input signals
   - Input and output test connections for monitoring of all critical points
- Onboard low-noise reference
   - Flagship LTC6655 for optimal performance
   - "Reference Corruption" input allows noise and interfering tones to be introduced and their effects observed
- Selectable conversion rates from 5 Msps to 40 Msps allows observation of aliasing, ADC noise density vs. sample rate

Applications
++++++++++++

- Mixed Signal and Digital Signal Processing experiments, workshops, and lab exercises
- Precision data acquisition

System Architecture
+++++++++++++++++++

.. figure:: adalm-mmsc_block_diagram.png

   ADALM-MMSC Simplified Block Diagram

What's Inside the Box?
++++++++++++++++++++++

Upon purchase of the ADALM-MMSC kit, the package comes with the following boards and accessories:

.. figure:: package_contents.png

   ADALM-MMSC Kit Contents

Package Contents
++++++++++++++++

- A: 1x ADALM-MMSC Board
- B: 1x Pre-programmed MAX32666FTHR board
- C: 1x USB-A to Micro-USB cable
- D: 1x MAX32625PICO with 10-pin ribbon cable for SWD connection (for future firmware upgrades)

Components and Connections
++++++++++++++++++++++++++

.. csv-table:: Jumper Configurations
   :file: jumper_configurations.csv
   :widths: 5, 25, 60
   :header-rows: 1

.. csv-table:: Connection Descriptions
   :file: connections.csv
   :widths: 5, 25, 60
   :header-rows: 1

.. figure:: primary_side.png

   ADALM-MMSC Primary Side

.. figure:: secondary_side.png

   ADALM-MMSC Secondary Side
   
Quick Start
+++++++++++

There are numerous combinations of signal sources, signal analyzers, and software that can be used with the ADALM-MMSC. The most straightforward to get up and running, and establish baseline functionality is to use the ADALM2000 with Scopy. Scopy is a general-purpose IIO-based utility, with instrument-specific control panels for various hardware, including the ADALM2000.


Equipment Needed
^^^^^^^^^^^^^^^^

- 1x ADALM-MMSC Board (as DUT)
- Windows, Mac, or Linux host computer with available USB port
- Signal generator, :adi:`ADALM2000 <https://analogdevicesinc.github.io/documentation/tools/m2k/index.html>` or similar.
   - Generators in the audio range work great
   - PC sound card with Audacity is another option


Software Needed
^^^^^^^^^^^^^^^

- Requires `Scopy 2.1 or newer <https://analogdevicesinc.github.io/scopy/index.html>`_.

- Connect the ADALM-MMSC to the host computer using the USB-A to USB-Micro cable, and open the Scopy GUI. If you are using the ADALM2000, connect the instrument and allow the calibration to complete. The next step is to add the ADALM-MMSC connection. Click the "+" button in the Scopy home screen as shown in the figure below.

.. figure:: scopy_add_device.png

   Adding the ADALM-MMSC in Scopy

- Click the "Scan" button, and select the ADALM-MMSC's serial port. There may be more than one serial port, depending on what other devices are connected to the host machine.

.. figure:: scopy_scan.png

   Scanning for the ADALM-MMSC Serial Port

- Click Verify, then Add Device.

.. figure:: scopy_verify.png

   Verifying the ADALM-MMSC Connection

- From the home screen, click Connect.

.. figure:: scopy_connect.png

   Connecting to the ADALM-MMSC in Scopy

- At this point Scopy is ready to acquire data from the ADALM-MMSC. Click into the ADC Time panel, set the buffer size to 4096 (maximum is =16384, limited by the AD4080's FIFO depth), then click Start. The figure below shows the ADC output when the signal generator is set to:

   - Channel 1 : 1 Vpp, 500 kHz Sinewave
   - Channel 2: 1 Vpp, 50 kHz Sawtooth

.. figure:: scopy_adc_time.png

   ADC Time Domain Capture in Scopy

- Refer to supporting lab exercises and workshop material for additional experiments.

Firmware Upgrades
+++++++++++++++++

The ADALM-MMSC firmware source is available at:
`ADALM-MMSC firmware (no-OS) <https://github.com/analogdevicesinc/no-OS/tree/main/projects/adalm-mmsc>`_

Pre-built binaries are available at:
`ADALM-MMSC releases <https://github.com/analogdevicesinc/no-OS/releases/tag/last_commit>`_
(Filename: ``adalm-mmsc.zip``)

The included MAX32625PICO is used to upgrade the firmware. Follow the directions at:
`MAX32625PICO firmware images <https://github.com/analogdevicesinc/max32625pico-firmware-images>`_
to load the MAX32666FTHR-specific binary.

Then, connect the 10-pin ribbon cable to the MAX32666FTHR, and drag-and-drop the
``adalm-mmsc_maxim_iio_example_max32665.hex`` file into the DAPLINK drive
(typically ``D:\`` or ``E:\`` on Windows). The drive will automatically eject
and the firmware will be upgraded.


Design & Integration Files
++++++++++++++++++++++++++

   * :download: `Schematic <doc/tools/adalm-mmsc/adalm-mmsc_revb_schematic.pdf>`_
   * :download: `PCB Layout <doc/tools/adalm-mmsc/adalm-mmsc_revb_pcb_layout.pdf>`_
   * :download: `Bill of Materials <doc/tools/adalm-mmsc/adalm-mmsc_revb_bom.csv>`_
   * :download: `Allegro Project <doc/tools/adalm-mmsc/adalm-mmsc_revb_allegro_files.zip>`_
   * Simulation Files

Help and Support
----------------

For questions and more information, please visit the :ez:`EngineerZone Support Community </>`.

Using ADALM-MMSC with Various ADI Software
++++++++++++++++++++++++++++++++++++++++++

Availlable software tools for use with the ADALM-MMSC include:
   * Device Drivers
   * Example Projects
   * Firmware

Connecting to IIO Server
Connecting to Scopy

Additional Resources
++++++++++++++++++++

   * Links to related demo or example projects 
   * Product pages
   * Application notes
   * Links to other demo poages using the board
   * Scopy (`Analog Devices Wiki <https://wiki.analog.com/university/tools/m2k/scopy>`_)
