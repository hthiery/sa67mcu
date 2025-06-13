

# Register

| Offset | Description           |
| ------ | --------------------- |
| 0x00   | Configuration control |
| 0x01   | Configuration offset  |
| 0x02   | Configuration data    |
| 0x03   | Version               |
| 0x04   | Watchdog              |
| 0x05   | Watchdog              |
| 0x06   | Watchdog              |
| 0x07   | Watchdog              |
| 0x10   | Bootmode              |
| 0x11   | Bootmode              |
| 0xff   | Debug                 |


## Configuration

### Control

| Value | Control      |
| ----- | ------------ |
| 1     | load config  |
| 2     | save config  |
| 4     | erase config |

### Data

| Offset | Description  |
| ------ | ------------ |
| 0      | version      |
| 1      | flags LSB    |
| 2      | flags MSB    |
| 3      | bootmode LSB |
| 4      | bootmode MSB |
| 5      | reserved0    |
| 6      | reserved1    |
| 7      | reserved2    |

### Flags

| Offset | Description               |
| ------ | ------------------------- |
| 0      | INITIAL_PWR_OFF           |
| 1      | DRIVE_BOOTMODE            |
| 2      | ENABLE_WATCHDOG           |
| 3      | DISABLE_FAILSAFE_WATCHDOG |
| 4..14  | reserved                  |
| 15     | DEBUG                     |


## Config Examples

### Example set bootmode (primary DFU and backup SPI)

From u-boot CLI:

    => i2c dev 2
    => i2c mw 4a 0 1   # control: load config
    => i2c mw 4a 1 1   # offset:  flags LSB
    => i2c mw 4a 2 2   # data:    set drive bootmode flag
    => i2c mw 4a 1 3   # offset:  bootmode LSB
    => i2c mw 4a 2 53  # data:    write value
    => i2c mw 4a 1 4   # offset:  bootmode MSB
    => i2c mw 4a 2 30  # data:    write value
    => i2c mw 4a 0 2   # control: save config

### Example clear all flags

From u-boot CLI:

    => i2c dev 2
    => i2c mw 4a 0 1   # control: load
    => i2c mw 4a 1 1   # offset:  flags LSB
    => i2c mw 4a 2 0   # data:    set all values to 0
    => i2c mw 4a 1 2   # offset:  flags MSB
    => i2c mw 4a 2 0   # data:    set all values to 0
    => i2c mw 4a 0 2   # control: save


### Example get bootmode

From u-boot CLI:

    => i2c dev 2
    => i2c mw 4a 0 1   # control: load
    => i2c mw 4a 1 3   # offset:  bootmode LSB
    => i2c md 4a 2 1   # data:    read value
    => i2c mw 4a 1 4   # offset:  bootmode MSB
    => i2c md 4a 2 1   # data:    read value

### Example erase config

From u-boot CLI:

    => i2c dev 2
    => i2c mw 4a 0 4   # control: erase
