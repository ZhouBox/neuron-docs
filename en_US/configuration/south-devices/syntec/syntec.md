# SYNTEC CNC

The SYNTEC CNC driver accesses the SYNTEC CNC system through the TCP protocol, enabling real-time collection of various equipment operation data, including program names, spindle override, operation status, PLC points, parameters, global variables, etc.

## Device Settings

| Field    | Description                    |
| -------- | ------------------------------ |
| host     | Hub IP address                 |
| port     | Hub port number, default 18090 |
| nodename | Hub node name                  |

::: tip
Due to the unique nature of SYNTEC CNC connections, a Hub conversion program is required. For specific acquisition and usage, please [contact us](https://www.emqx.com/en/contact?product=neuron).
:::

## Supported Data Types

* uint8
* int8
* uint32
* int32
* uint64
* int64
* float
* double
* bit
* string

## CNC Data

> address\[.m]\[.n]

| Tag Identifier (Address) | Description                      | Data Type    | Parameters | Notes                                                                                                                                                                                   |
| ------------------------ | -------------------------------- | ------------ | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| systemInfo.axes          | Controllable axes count          | int32        | -          | -                                                                                                                                                                                       |
| systemInfo.cnc_type      | System type                      | string       | -          | -                                                                                                                                                                                       |
| systemInfo.max_axes      | Maximum axes count               | int32        | -          | -                                                                                                                                                                                       |
| systemInfo.series        | M/T type                         | string       | -          | -                                                                                                                                                                                       |
| systemInfo.nc_ver        | System software version          | string       | -          | -                                                                                                                                                                                       |
| systemInfo.axis_name     | Axis coordinate names            | array string | -          | -                                                                                                                                                                                       |
| systemStatus.status      | Current operation status         | string       | -          | Status                                                                                                                                                                                  |
| systemStatus.main_prog   | Main program name                | string       | -          | 0: Input mode 1: Automatic mode 2: Invalid mode 3: Edit mode 4: Step mode 5: Manual mode 8: Handwheel mode 9: Mechanical zero return mode 10: Program zero return mode                  |
| systemStatus.cur_prog    | Currently executing program name | string       | -          | -                                                                                                                                                                                       |
| systemStatus.mode        | Mode                             | string       | -          | 0x1: Emergency stop signal active 0x2: Servo not ready 0x4: IO not ready (remote IO devices, etc.)                                                                                      |
| systemStatus.alarm       | Alarm                            | string       | -          | -                                                                                                                                                                                       |
| systemStatus.emg         | Emergency stop                   | string       | -          | -                                                                                                                                                                                       |
| alarms                   | Current alarms                   | array string | -          | -                                                                                                                                                                                       |
| halarms                  | Historical alarms                | array string | -          | -                                                                                                                                                                                       |
| oplog                    | Operation logs                   | array string | -          | -                                                                                                                                                                                       |
| absolute                 | Absolute coordinates             | double       | m          | X Y Z                                                                                                                                                                                   |
| machine                  | Machine coordinates              | double       | m          | X Y Z                                                                                                                                                                                   |
| relative                 | Relative coordinates             | double       | m          | X Y Z                                                                                                                                                                                   |
| distance                 | Remaining distance               | double       | m          | X Y Z                                                                                                                                                                                   |
| time                     | Time                             | int32        | m          | power: Power-on time (unit: seconds) cutting: Cutting time (unit: seconds) cycle: Cycle time (unit: seconds) work: Processing time (unit: seconds)                                      |
| partCount                | Processing count                 | int32        | m          | total: Total processing count cur: Current processing count cur: Required processing count                                                                                              |
| ovfeed                   | Current feed override            | double       | -          | -                                                                                                                                                                                       |
| ovspindle                | Current spindle override         | double       | -          | -                                                                                                                                                                                       |
| actfeed                  | Actual feed rate                 | double       | -          | -                                                                                                                                                                                       |
| actspindle               | Actual spindle speed             | int32        | -          | -                                                                                                                                                                                       |
| gcode                    | G code                           | array string | -          | -                                                                                                                                                                                       |
| otherCode                | Other codes                      | int32        | m          | hcode   dcode   mcode    tcode   fcode    scode                                                                                                                                         |
| macro                    | Global variables                 | double       | m          | Global variable number                                                                                                                                                                  |
| param                    | Parameters                       | int32        | m          | Parameter number                                                                                                                                                                        |
| toolOffset               | Tool compensation                | double       | m，n       | m: Tool compensation number. n: RADIUS_GEOM, RADIUS_WEAR, LENGTH_GEOM, LENGTH_WEAR, WEAR_X, WEAR_Z, WEAR_A, LENGTH_X, LENGTH_Y, LENGTH_A, TOOL_NOSE_RADIUS, TOOL_NOSE_R_WEAR, TOOL_NOSE |

::: tip
macro and param are readable and writable, others are read-only.
:::

### CNC Address Examples

| Address                 | Description                              |
| ----------------------- | ---------------------------------------- |
| systemInfo.nc_ver       | Read system software version             |
| machine.X               | Read X-axis coordinate                   |
| macro.100               | Read/write global variable 100           |
| feedOverride            | Read current feed override               |
| alarms                  | Current alarms                           |
| partCount.total         | Total processing count                   |
| toolOffset1.RADIUS_GEOM | Tool compensation 1, radius compensation |

### PLC Data

#### Address Format

> AREA ADDRESS\[.BIT]\[.LEN]

| Identifier | Description | Type                                              | Permission |
| ---------- | ----------- | ------------------------------------------------- | ---------- |
| I          | Input Bits  | bit/int8/uint8                                    | Read       |
| O          | Output Bits | bit/int8/uint8                                    | Read       |
| C          | C Bits      | bit/int8/uint8                                    | Read       |
| S          | S Bits      | bit/int8/uint8                                    | Read       |
| A          | A Bits      | bit/int8/uint8                                    | Read       |
| R          | Registers   | bit/int32/uint32/int64/uint64/float/double/string | Read/Write |

::: tip
Currently, only setting part of the R area is supported, bit writing is not supported.
:::

> AREA ADDRESS\[.setting/value/state]

| Identifier | Description | Type         | Permission |
| ---------- | ----------- | ------------ | ---------- |
| T          | Timer       | int32/uint32 | Read       |
| N          | Counter     | int32/uint32 | Read       |

::: tip
When reading timers and counters, you need to specify whether it is the setting value, current value, or status.
:::

#### Common PLC Points

| Address    | Type  | Description                            |
| ---------- | ----- | -------------------------------------- |
| I0.0       | bit   | Input Bits area, data at address 0     |
| A10        | int8  | A Bits area, data at address 10        |
| R100       | float | Register area, data at address 100     |
| T0.setting | int32 | Timer area, setting value at address 0 |
| T0.value   | int32 | Timer area, current value at address 0 |