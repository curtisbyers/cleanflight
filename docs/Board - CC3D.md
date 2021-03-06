# Board - CC3D

The OpenPilot Copter Control 3D aka CC3D is a board more tuned to Acrobatic flying or GPS based
auto-piloting.  It only has one sensor, the MPU6000 SPI based Accelerometer/Gyro.
It also features a 16Mbit SPI based EEPROM chip.  It has 6 ports labeled as inputs (one pin each)
and 6 ports labeled as motor/servo outputs (3 pins each).

If issues are found with this board please report via the [github issue tracker](https://github.com/cleanflight/cleanflight/issues).

The board has a USB port directly connected to the processor.  Other boards like the Naze and Flip32
have an on-board USB to uart adapter which connect to the processor's serial port instead.

Currently there is no support for virtual com port functionality on the CC3D which means that cleanflight
does not currently use the USB socket at all. Therefore, the communication with the board is achieved through a USB-UART adaptater connected to the Main port.

The board cannot currently be used for hexacopters/octocopters.

Tricopter & Airplane support is untested, please report success or failure if you try it. 

# Pinouts

The 8 pin RC_Input connector has the following pinouts when used in RX_PPM/RX_SERIAL mode

| Pin | Function  | Notes                            |
| --- | --------- | -------------------------------- |
| 1   | Ground    |                                  |
| 2   | +5V       |                                  |
| 3   | PPM Input | Enable `feature RX_PPM`          | 
| 4   | SoftSerial1 TX | Enable `feature SOFTSERIAL` |
| 5   | SoftSerial1 RX | Enable `feature SOFTSERIAL` |
| 6   | Current   | Enable `feature CURRENT_METER`.  Connect to the output of a current sensor, 0v-3.3v input |
| 7   | Battery Voltage sensor | Enable `feature VBAT`. Connect to main battery using a voltage divider, 0v-3.3v input |
| 8   | RSSI      | Enable `feature RSSI_ADC`.  Connect to the output of a PWM-RSSI conditioner, 0v-3.3v input |

The 6 pin RC_Output connector has the following pinouts when used in RX_PPM/RX_SERIAL mode

| Pin | Function  | Notes |
| --- | ----------| ------|
| 1   | MOTOR 1   |       |
| 2   | MOTOR 2   |       |
| 3   | MOTOR 3   |       |
| 4   | MOTOR 4   |       |
| 5   | LED Strip |       |
| 6   | Unused    |       |

The 8 pin RC_Input connector has the following pinouts when used in RX_PARALLEL_PWM mode

| Pin | Function | Notes |
| --- | ---------| ------|
| 1   | Ground   |       |
| 2   | +5V      |       |
| 3   | Unused   |       | 
| 4   | CH1      |       |
| 5   | CH2      |       |
| 6   | CH3      |       |
| 7   | CH4/Battery Voltage sensor      | CH4 if battery voltage sensor is disabled |
| 8   | CH5/CH4  | CH4 if battery voltage monitor is enabled|

The 6 pin RC_Output connector has the following pinouts when used in RX_PARALLEL_PWM mode

| Pin | Function | Notes |
| --- | ---------| ------|
| 1   | MOTOR 1  |       |
| 2   | MOTOR 2  |       |
| 3   | MOTOR 3  |       |
| 4   | MOTOR 4  |       |
| 5   | Unused   |       |
| 6   | Unused   |       |

# Serial Ports

| Value | Identifier   | Board Markings | Notes                                    |
| ----- | ------------ | -------------- | -----------------------------------------|
| 1     | USART1       | MAIN PORT      | Has a hardware inverter for S.BUS        |
| 2     | USART3       | FLEX PORT      |                                          |
| 3     | SoftSerial   | RC connector   | Pins 4 and 5 (Tx and Rx respectively)    |

The SoftSerial port is not available when RX_PARALLEL_PWM is used. The transmission data rate is limited to 19200 baud.

To connect the GUI to the flight controller you need additional hardware attached to the USART1 serial port (by default).

# Flashing

There are two primary ways to get Cleanflight onto a CC3D board.

* Single binary image mode - best mode if you don't want to use OpenPilot.
* OpenPilot Bootloader compatible image mode - best mode if you want to switch between OpenPilot and Cleanflight.

## Single binary image mode.

The entire flash ram on the target processor is flashed with a single image.

## OpenPilot Bootloader compatible image mode.

The initial section of flash ram on the target process is flashed with a bootloader which can then run the code in the
remaining area of flash ram.

The OpenPilot bootloader code also allows the remaining section of flash to be reconfigured and re-flashed by the
OpenPilot Ground Station (GCS) via USB without requiring a USB to uart adapter.

In this mode a USB to uart adapter is still required to connect to via the GUI or CLI.
