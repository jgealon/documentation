ADALM-LSMSPG
============

Active Learning Module for Low-Speed Mixed-Signal Exploration and Prototyping.

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

.. figure:: tools/adalm-lsmspg/adalm-lsmspg_block_diagram.png

   ADALM-LSMSPG Simplified Block Diagram

Specifications
++++++++++++++


What's Inside the Box?
++++++++++++++++++++++

Upon purchase of the ADALM-LSMSPG kit, the package comes with the following boards and accessories:

.. figure:: adalm-lsmspg_package_contents.png

   ADALM-LSMSPG Kit Contents

Package Contents
++++++++++++++++

- ADALM-LSMSPG main board
- 40-pin, 15 cm ribbon cable for Raspberry Pi
- Jumpers for setting AD5592r and LM75 I2C address

Components and Connections
++++++++++++++++++++++++++

.. figure:: adalm-lsmspg_with_pin_labels.png

   ADALM-LSMSPG Hardware Components and Connections


- **P31**: Raspberry Pi interface. Connect to Raspberry Pi 4, 400, 5, 500 via supplied ribbon cable. Be sure to observe polarity / pin 1 locaton.

- **P1**, **P28**: Feather interface. Pins or stacking headers must be installed on Feather platform board, with pins facing downward.

*Note:* The Raspberry Pi and Feather interfaces are mutually exclusive.

- **P16**: ADALM2000 interface. Allows monitoring of digital traffic on the I2C, SPI, and various GPIO signals.

- **P27**: ADALM2000 through-connections. Allows monitoring of other signals via jumper wires.

Refer to the table below for P16, P27 signal mapping.

.. csv-table:: P16 and P27 Signal Mapping
   :file: p16-p27_signal_mapping.csv
   :widths: 30, 30
   :header-rows: 1

- P31:  AD5592r GPIO, Reference connections
- P5 : AD5593r GPIO, Reference connections 

Refer to the figure below for P2, P5 signal mapping.
.. figure:: ad559xr_gpio_pin.png

   P2 and P5 Signal Mapping

Address Selection Jumpers
+++++++++++++++++++++++++
.. csv-table:: P4 Address Selection Jumpers
   :file: ad5593r_i2c_address.csv
   :widths: 30, 30
   :header-rows: 1

.. csv-table:: P9 Address Selection Jumpers
   :file: lm75_i2c_address.csv
   :widths: 30, 30
   :header-rows: 1

System Monitor/Control
++++++++++++++++++++++

- DS1: Heartbeat LED
- S1 : Raspberry Pi Shutdown (set in config.txt), Feather reset

Curve Tracer Application Circuits
+++++++++++++++++++++++++++++++++

The AD5592r and AD5593r drive simple NPN and PNP (respectively) curve tracer circuits. 
This represents a simple DC test instrument that involves the "set a current, set a voltage, take a measurement, do some math, step, repeat" sequence of operations. 
The details of the experiments are covered in the `Tools for LSMSPG Lesson <https://analogdevicesinc.github.io/documentation/learning/tools_for_ls/index.html>`.

.. figure:: ad5592r_npn_curve_tracer_connections.png

   AD5592r NPN Curve Tracer Connections 

The PNP curve tracer has two options for the emitter drive: AD5593r CH3 (default), or the 3.3V supply rail.

.. figure:: ad5593r_pnp_curve_tracer_connections.png

   AD5593r PNP Curve Tracer Connections 

Hardware Setup
++++++++++++++

Raspberry Pi Setup:

For Raspberry Pi Zero 2 W, 4, and 5, use the included ribbon cable to make the connection as shown in Figure 1

<<Figure 1 - either cartoon or photo of ribbon cable connection>>

For Raspberry Pi 400 and 500, use the included ribbon cable to make the connection as shown in Figure 2

<<Figure 2 - either cartoon or photo of ribbon cable connection>>

Feather Setup:

Most Feather platform boards ship without peripheral headers installed. Refer to the board's instructions, and install either:

Downward-facing 100-mil post headers
Stacking headers with posts extending from the reverse side of the board.
Install the Feather on P20 as shown in Figure 3.

<<Figure  - either cartoon or photo of Feather mounted to LSMSPG>>

Software Setup
++++++++++++++

Raspberry Pi Setup
^^^^^^^^^^^^^^^^^^

Prepare an SD card with ADI Kuiper Linux following the instructions at `ADI Kuiper Linux Guide <https://analogdevicesinc.github.io/documentation/linux/kuiper/index.html>`.
Add the following to the end of ``/boot/config.txt``:

.. code-block:: none

   # config.txt additions

   dtoverlay=rpi-adalm-lsmspg

   # Heartbeat blinky:
   dtparam=act_led_gpio=20
   dtparam=act_led_trigger=heartbeat

   # Short GPIO21 (pin 40) to ground for shutdown:
   dtoverlay=gpio-shutdown,gpio_pin=21,active_low=1,gpiopull=up


With the ADALM-LSMSPG connected, the heartbeat LED will begin beating on boot.
The shutdown button is a convenient means of shutting down the Raspberry Pi
properly in headless situations (no local monitor or keyboard). Press the
shutdown button, wait 5 seconds after the heartbeat LED stops blinking, and
then it is safe to remove power.

MAX32666FTHR Setup
^^^^^^^^^^^^^^^^^^

The `MAX32666FTHR <https://www.analog.com/MAX32666FTHR>` includes a
MAX32625PICO debugger that can be used to load the ADALM-LSMSPG firmware image.
Prepare the MAX32625PICO itself with the MAX32666FTHR-specific DAPLink image from:

`MAX32625PICO firmware images <https://github.com/analogdevicesinc/max32625pico-firmware-images>`

Leave the MAX32625PICO plugged in, and plug in the MAX32666FTHR using another
USB-Micro cable. Connect the debug interface to the MAX32666FTHR with the supplied
10-pin ribbon cable. Download the ADALM-LSMSPG tinyiiod firmware image
(filename ``adalm-lsmspg.zip``) from:

`ADALM-LSMSPG firmware (no-OS releases) <https://github.com/analogdevicesinc/no-OS/releases/tag/last_commit>`

Unzip the archive, copy the ``adalm-lsmspg_maxim_iio.hex`` file, and paste it into
the DAPLINK mass storage device (typically ``D:`` or ``E:`` on Windows systems).
The DAPLINK drive will auto-eject, and the heartbeat LED on the ADALM-LSMSPG
will begin blinking.

Application Setup
^^^^^^^^^^^^^^^^^

Detailed instructions for various application software examples are included in the
`Tools for Low-Speed Mixed Signal workshop <https://analogdevicesinc.github.io/documentation/learning/tools_for_ls/index.html>`,
as well as other supporting material. The underlying API for Raspberry Pi/Linux and
bare-metal tinyiiod servers is ``libiio``, which can be installed from: `libiio documentation <https://analogdevicesinc.github.io/libiio/index.html>`

Once installed, information about the ADALM-LSMSPG context can be displayed by running ``iio_info`` and passing the URI argument.

For Kuiper Linux, the default hostname is ``analog``. Example:

.. code-block:: none

   iio_info - network

   iio_info -s ip:analog.local

For the MAX32666FTHR running the tinyiiod firmware, run:

.. code-block:: none

   iio_info - serial

   iio_info -s serial:COM4

Replace ``COM4`` with the appropriate COM port number on Windows, or ``/dev/ttyX`` on Linux.

The *Tools for Low-Speed Mixed-Signal System Design* tutorial contains many additional examples of communicating with the ADALM-MMSC.

Additional Resources
++++++++++++++++++++

- `Tools for Low-Speed Mixed Signal Workshop <https://analogdevicesinc.github.io/documentation/learning/tools_for_ls/index.html>`
- `AD5592R Product Page <https://analog.com/ad5592r>`
- `AD5593R Product Page <https://analog.com/ad5593r>`
- `LM75 Product Page <https://analog.com/lm75>`

Design & Integration Files
++++++++++++++++++++++++++

- Schematic
- PCB Layout
- Bill of Materials
- Allegro Project

Help and Support
----------------

For questions and more information, please visit the :ez:`EngineerZone Support Community </>` or contact your local ADI representative.

