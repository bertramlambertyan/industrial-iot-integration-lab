# Banner K30 Modbus RTU Test

This case documents a practical Modbus RTU test for Banner K30 touch indicator lights.

The purpose of this note is to record the wiring, communication settings, register mapping, troubleshooting process, and lessons learned during the integration test.

This is a field-based lab note, not a finished product document.

## Background

The test was created to verify whether a WAGO PLC can control and read Banner K30 touch indicator lights through Modbus RTU.

The main test goals were:

- Control light status through Modbus write commands
- Read touch button status through Modbus read commands
- Verify register address mapping
- Compare communication stability between different serial interfaces
- Prepare the logic for multi-device door access control applications

## Goal

Build a stable Modbus RTU communication flow between WAGO PLC and Banner K30 devices.

The final goal is to support a multi-station system where each K30 device can be used as an indicator and touch input device.

## Hardware

| Item | Description |
|---|---|
| PLC | WAGO PFC200 750-8212 |
| Serial Module | WAGO 750-653 RS-485 module |
| Device | Banner K30PTSQ touch indicator light |
| Communication | RS-485 / Modbus RTU |
| Test Tools | Modscan / Modbus Poll / CODESYS |
| Debug Tools | Ping / Wireshark / Serial test tools where needed |

## Software

- CODESYS 3.5
- Modscan or Modbus Poll
- WAGO libraries for serial / Modbus communication
- Optional tools for troubleshooting:
  - Wireshark
  - Docklight
  - PowerShell / CMD

## Wiring

The RS-485 wiring used in the test:

| Signal | Wiring |
|---|---|
| D+ | Pin 1 / Pin 2 |
| D- | Pin 5 / Pin 6 |

Basic wiring concept:

```text
WAGO PLC / Serial Module
        ↓ RS-485
Banner K30 Modbus RTU Device
```

## Communication Settings

The tested Modbus RTU settings:

| Parameter | Value |
|---|---|
| Protocol | Modbus RTU |
| Baud Rate | 19200 |
| Data Bits | 8 |
| Parity | None |
| Stop Bits | 1 |
| Function Code for Read | FC03 |
| Function Code for Write | Modbus write command, depending on library function |
| Main Test Slave IDs | 1, 2 |
| Planned Slave IDs | 1 to 16 |

Notes:

- FC03 was used for reading holding registers.
- FC04 was tested but did not work for the target registers.
- 9600 baud rate was also tested, but 19200 / 8 / N / 1 became the main test setting.

## Important Address Mapping

The device manual uses Base 1 addressing, but the program uses Base 0 addressing.

This means:

```text
Manual Address - 1 = Program Address
```

Example:

```text
Manual 8701 = Program 8700
```

## Register Mapping

| Purpose | Manual Address | Program Address | Notes |
|---|---:|---:|---|
| Job / Light Control 1 | 8701 | 8700 | Used for writing light job data |
| Job / Light Control 2 | 8702 | 8701 | Used with job settings |
| Job / Light Control 3 | 8703 | 8702 | Used with job settings |
| Touch Status | 7942 | 7941 | Read touch trigger status |
| Polling / Report Test | 6101 | 6100 | Used during report / polling tests |

Important note:

Some tools display addresses using Base 1 format, while PLC programs usually use Base 0 format.  
This difference can easily cause wrong reads or writes.

## Test Procedure

### 1. Verify Device Communication with Modbus Tool

Use Modscan or Modbus Poll to confirm that the Banner K30 can respond correctly.

Checklist:

- Correct COM port
- Correct baud rate
- Correct slave ID
- Correct function code
- Correct register address
- Correct RS-485 wiring

### 2. Test Read Function

Read the touch status register.

Main target:

```text
Manual Address: 7942
Program Address: 7941
```

Expected result:

- When the K30 touch button is pressed, the read value should change.
- The short press signal may require proper polling timing.

### 3. Test Write Function

Write light job values to the job registers.

Main target:

```text
Manual Address: 8701 to 8703
Program Address: 8700 to 8702
```

Expected result:

- The light status should change after writing the correct value.
- The written value should be read back for verification.

### 4. Compare Serial Interfaces

Two communication paths were compared:

| Interface | Result |
|---|---|
| PFC200 built-in COM1 | Communication was unstable during the test |
| WAGO 750-653 RS-485 module | Communication became more stable |

## Observed Results

### Modbus Tool Result

Modscan / Modbus Poll could communicate with the Banner K30 normally when the settings were correct.

### PLC Built-in COM1 Result

The PFC200 built-in COM1 communication was not stable enough during the test.

Observed issues included:

- Disconnection
- Slow response
- Timeout
- Unstable read behavior

### WAGO 750-653 Result

After changing to the WAGO 750-653 RS-485 module, the communication became much more stable.

Observed improvements:

- More stable polling
- Better response timing
- Lower communication failure rate
- More reliable touch status reading

## Troubleshooting Notes

### Problem 1: Address Offset

The manual address and program address were different.

Solution:

Use Base 0 conversion.

```text
Manual Address - 1 = Program Address
```

### Problem 2: Wrong Function Code

FC04 was tested but did not work for the target registers.

Solution:

Use FC03 for holding register reads.

### Problem 3: Touch Status Not Read Correctly

The touch signal was not always captured at first.

Possible causes:

- Wrong address
- Polling timing too slow
- Short touch signal duration

Solution:

- Use the correct Base 0 address
- Use FC03
- Improve polling logic
- Trigger read request more clearly in the PLC program

### Problem 4: Built-in COM1 Instability

The built-in COM1 interface on the PFC200 was unstable during this test.

Solution:

Use the WAGO 750-653 RS-485 module for more stable communication.

## Application Logic Concept

The planned application is a 16-station door access control system.

Each station includes:

- One Banner K30 touch indicator
- One request input
- One door open signal
- One unlock output
- One alarm output

## Example Door Access Flow

```text
Request DI ON
        ↓
K30 green light ON
        ↓
Wait for touch button
        ↓
Unlock DO ON
        ↓
Door open
        ↓
K30 red light ON
        ↓
Door close and lock
        ↓
K30 light OFF
```

## Planned 16-Station Logic

The final concept includes 16 K30 devices with Modbus slave IDs from 1 to 16.

Planned behavior:

1. DI request is triggered.
2. The related K30 turns green.
3. If the button is not pressed within the timeout, alarm output is triggered.
4. If the button is pressed, unlock output is enabled.
5. When the door opens, the K30 changes to red.
6. If the door remains open too long, alarm output is triggered.
7. When the door closes and locks, the K30 turns off.

## Lessons Learned

### 1. Confirm with Modbus Tools First

Before writing PLC code, use Modscan or Modbus Poll to confirm that the device responds correctly.

### 2. Always Check Base 0 / Base 1 Addressing

Address offset issues are common in Modbus integration.

### 3. Function Code Matters

The correct register type and function code must be confirmed.

### 4. Serial Interface Stability Matters

Even when the protocol settings are correct, hardware interface stability can still affect the result.

### 5. Readback Is Important

After writing values to the device, read back the register value to confirm the command was accepted.

### 6. Polling Timing Affects Short Signals

For touch buttons or short trigger signals, polling speed and state handling are important.

## Future Improvements

- Add wiring diagram
- Add CODESYS program structure
- Add Modbus polling scheduler example
- Add screenshots from Modbus Poll / Modscan
- Add register table from the device manual
- Add multi-device polling test result
- Add 16-station control logic documentation
- Add alarm and timeout timing diagram

## Related Topics

- Modbus RTU
- RS-485
- WAGO PFC200
- WAGO 750-653
- Banner K30PTSQ
- Base 0 / Base 1 addressing
- Industrial field device integration
- Door access control
- OT/IT integration
