# Fanuc Focas Ethernet

The Neuron FOCAS driver supports the use of the FOCAS2 protocol for data acquisition and writing for various types of FANUC devices (including multi-path devices). It supports coordinates, operating status, alarms, operation information, macro variables, parameters, diagnostic data, program data, tool offset, tooling, tool life, PMC, and more.

**Support arch**: amd64, armv7

## Parameter Configuration

| Parameter | Description                        |
| --------- | ---------------------------------- |
| host      | device ip address                  |
| port      | device port, default 8193          |
| timeout   | connection timeout, default 3000ms |

## Support Data Type

* uint8
* int8
* uint16
* int16
* uint32
* int32
* uint64
* int64
* float
* double
* bit
* string

## CNC Data

### Address Format
`address[.m][.n][.l][@p]`

The address can support up to 3 parameters, with the `@` symbol specifying the channel where the address is located.

| Tag Identifier (Address) | Description                                      | Data Type           | Parameters                                                                 | Remarks                                                                                                                                                                                                                        |
| ------------------------ | ------------------------------------------------ | ------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| actf                     | Actual feed rate                                 | int64/uint64        | -                                                                          |                                                                                                                                                                                                                                |
| absolute.m               | Axis absolute position                           | int64/uint64        | Axis number                                                                | Includes tool offset, may not match the display on the CNC                                                                                                                                                                     |
| absolute2.m              | Axis absolute position 2                         | int64/uint64        | Axis number                                                                | Data is consistent with the CNC interface                                                                                                                                                                                      |
| machine.m                | Axis machine position                            | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| relative.m               | Axis relative position                           | int64/uint64        | Axis number                                                                | Includes tool offset, may not match the display on the CNC                                                                                                                                                                     |
| relative2.m              | Axis relative position 2                         | int64/uint64        | Axis number                                                                | Data is consistent with the CNC interface                                                                                                                                                                                      |
| distance.m               | Axis remaining distance                          | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| acts                     | Actual spindle speed                             | int64/uint64        | -                                                                          |                                                                                                                                                                                                                                |
| acts2.m                  | Actual spindle speed 2                           | int64/uint64        | Spindle number                                                             |                                                                                                                                                                                                                                |
| skip                     | Axis skip position                               | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| srvdelay                 | Axis servo delay                                 | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| accdecdly                | Axis acceleration/deceleration delay             | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| spcss_srpm               | Spindle speed conversion                         | int64/uint64        | -                                                                          |                                                                                                                                                                                                                                |
| spcss_sspm               | Specified surface speed                          | int64/uint64        | -                                                                          |                                                                                                                                                                                                                                |
| spcss_smax               | Maximum spindle speed for the fixture            | int64/uint64        | -                                                                          |                                                                                                                                                                                                                                |
| movrlap_input.m          | Input overlapping motion value                   | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| movrlap_output.m         | Output overlapping motion value                  | int64/uint64        | Axis number                                                                |                                                                                                                                                                                                                                |
| spload.m                 | Spindle load                                     | int32/uint32        | Spindle number                                                             |                                                                                                                                                                                                                                |
| spmaxrpm.m               | Maximum spindle speed ratio                      | int32/uint32        | Spindle number                                                             |                                                                                                                                                                                                                                |
| spgear.m                 | Spindle gear ratio                               | int32/uint32        | Spindle number                                                             |                                                                                                                                                                                                                                |
| runState                 | Operating state                                  | int32/uint32        | -                                                                          | 1: Fault 2: Running 3: Idle                                                                                                                                                                                                    |
| controlMode              | Control mode                                     | int32/uint32        | -                                                                          | 0: MID 1: AUTO 3: EDIT 5: JOB 8: INC feed 9: REFERENCE 10: ReMoTe                                                                                                                                                              |
| programMain              | Machining program name                           | string              | -                                                                          |                                                                                                                                                                                                                                |
| mainProgramNo            | Main program number                              | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| subProgramNo             | Sub program number                               | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| alarm                    | Alarm ID                                         | int32/uint32        | -                                                                          | 0: WS 1: PW 2: IO 3: PS                                                                                                                                                                                                        |
| alarmMsg.m               | Alarm content                                    | string              | m as number                                                                | Range is 1-16                                                                                                                                                                                                                  |
| param.m.n                | Parameter                                        | int32/uint32        | m as parameter ID, n as axis number                                        | If not axis-related, set to 0                                                                                                                                                                                                  |
| alarmMsg2.m.n            | Alarm content 2                                  | int16/uint16/string | m as number, n as value parameter                                          | m range is 1-16, n=0 for alarm type, n=1 for alarm number, n=2 for alarm content                                                                                                                                               |
| macroType                | Macro variable type                              | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| macroInfo.m              | Macro variable information                       | int16/uint16        | n as value parameter                                                       | n=0 for number of local variables, n=1 for public variable indicator                                                                                                                                                           |
| macro.m.n                | Macro variable                                   | int32/uint32        | m as variable ID, n as value parameter                                     | n=0 for macro variable value, n=1 for decimal places                                                                                                                                                                           |
| paraNum.m                | Number of parameters                             | int16/uint16        | n as value parameter                                                       | n=0 for minimum number of parameters, n=1 for maximum number, n=3 for total number of parameters                                                                                                                               |
| pitchInfoNum.m           | Number of pitch error compensation data          | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| pitch.m                  | Pitch error compensation data                    | int8/uint8          | m as number                                                                |                                                                                                                                                                                                                                |
| tool.m.n                 | Tool data                                        | int64/uint64        | m as tool number, n as value variable                                      | m starts from 1, n=0 for tool number, n=1 for usage count, n=2 for tool life, n=3 for rest life                                                                                                                                |
| toolf2.m.n               | Tool data 2                                      | int64/uint64        | m as tool number, n as value variable                                      | m starts from 1, n=0 for tool number, n=1 for usage count, n=2 for tool life, n=3 for rest life                                                                                                                                |
| toolOffset.m.n           | Tool offset data                                 | int32/uint32        | m as tool number, n as tool offset type                                    | m starts from 1, n is related to the specific system model, e.g., for 0i-D, n=0 for radius wear, n=1 for radius shape, n=2 for length wear, n=3 for length shape                                                               |
| toolOffsetRange.m.n.l    | Tool offset data setting range                   | int32/uint32        | m as tool number, n as tool offset type, l as value type                   | m starts from 1, n is related to the specific system model, e.g., for 0i-D, n=0 for radius wear, n=1 for radius shape, n=2 for length wear, n=3 for length shape, l=0 for minimum value, l=1 for maximum value, l=2 for status |
| toolOffsetInfo.m         | Tool offset information                          | int16/uint16        | m as tool offset type                                                      | m=0 for memory type, m=1 for the number of available tool offsets, n=2 for tool offset types                                                                                                                                   |
| wkcdsfms.m               | Workpiece coordinate offset measurement value    | int32/uint32        | Axis number                                                                | Not supported in M series                                                                                                                                                                                                      |
| wkcdshft.m               | Workpiece coordinate offset value                | int32/uint32        | Axis number                                                                | Not supported in M series                                                                                                                                                                                                      |
| wksftRange.m             | Workpiece coordinate offset value range          | int32/uint32        | n as value parameter                                                       | m=0 for minimum value, m=1 for maximum value, m=2 for type, Not supported in M series                                                                                                                                          |
| zofs                     | Workpiece zero offset value                      | int32/uint32        | -                                                                          |                                                                                                                                                                                                                                |
| zofsInfoNum              | Number of workpiece zero offset values           | int32/uint32        | -                                                                          |                                                                                                                                                                                                                                |
| zofsRange.m.n.l          | Workpiece zero offset setting range              | int32/uint32        | m as workpiece coordinate offset number, n as axis number, l as value type | l=0 for minimum value, l=1 for maximum value, l=2 for status                                                                                                                                                                   |
| blkCount                 | Block counter                                    | int32/uint32        | -                                                                          | -                                                                                                                                                                                                                              |
| execProg                 | Currently truly executed program segment         | string              | -                                                                          |                                                                                                                                                                                                                                |
| mdiPntr                  | MDI execution information                        | int32/uint32        | -                                                                          | Readable only when the device is in MDI mode                                                                                                                                                                                   |
| programMainFile          | Main program file information                    | string              | -                                                                          |                                                                                                                                                                                                                                |
| pdfCurDir                | Current program directory                        | string              | -                                                                          |                                                                                                                                                                                                                                |
| pdfDrive.m               | Program device information                       | string              | m as device number                                                         | m starts from 1                                                                                                                                                                                                                |
| progInfo.m               | Program management information                   | int32/uint32        | m as value parameter                                                       | m=0 for number of registered programs, m=1 for number of available programs, m=2 for used memory character count, m=3 for unused memory character count                                                                        |
| seqNum                   | Program sequence number                          | int32/uint32        | -                                                                          |                                                                                                                                                                                                                                |
| exaxisName.m.n           | Control axis and spindle name                    | string              | m as axis type, n as number                                                | m=0 for control axis, m=1 for spindle, n starts from 1                                                                                                                                                                         |
| axisName.m               | Control axis name                                | int8/uint8          | m as number                                                                | m starts from 1                                                                                                                                                                                                                |
| hndintrpt.m.n            | Handwheel interrupt value                        | int32/uint32        | m as type, n as number                                                     | m=0 for input, m=1 for output, n starts from 1                                                                                                                                                                                 |
| spdlName.m               | Spindle name                                     | int8/uint8          | m as number                                                                | m starts from 1                                                                                                                                                                                                                |
| spmeter.m.n              | Spindle load percentage                          | int32/uint32        | m as type, n as number                                                     | m=0 for spindle load, m=1 for spindle motor load, n starts from 1                                                                                                                                                              |
| svmeter.m                | Servo load percentage                            | int32/uint32        | m as number                                                                | m starts from 1                                                                                                                                                                                                                |
| almhisno                 | Number of historical alarms                      | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| almhistry.m.n            | Historical alarm content                         | int16/uint16/string | m as number, n as value parameter                                          | m starts from 1, n=0 for alarm type, n=1 for alarm number, n=2 for alarm content                                                                                                                                               |
| omhisno                  | Number of historical extra operation information | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| omhistry.m.n             | Historical extra operation information content   | int16/uint16/string | m as number, n as value parameter                                          | m starts from 1, n=0 for display flag, n=1 for operation information number, n=2 for operation information content                                                                                                             |
| ophisno                  | Number of historical operation information       | int16/uint16        | -                                                                          |                                                                                                                                                                                                                                |
| ophistry.m.n             | Historical operation information content         | int16/uint16        | m as number, n as value parameter                                          | m starts from 1                                                                                                                                                                                                                |
| timer.m.n                | Date and time                                    | int16/uint16        | m as type, n as value parameter                                            | m=0 for date, n=0 for year, n=1 for month, n=2 for day; m=1 for time, n=0 for hour, n=1 for minute, n=2 for second                                                                                                             |
| diagdata.m.n             | Diagnostic data                                  | int32/uint32        | m as diagnostic number, n as value parameter                               | n=0 for diagnostic value, n=1 for decimal point                                                                                                                                                                                |
| opmsg.m.n                | Current operation information                    | int16/uint16/string | m as number, n as value parameter                                          | m starts from 1, n=0 for type, n=1 for number, n=2 for content                                                                                                                                                                 |
| tlGrpinfo.m.n            | Tool life management information                 | int32/uint32        | m as tool group number, n as value parameter                               | m starts from 1, n=0 for tool quantity, n=1 for remaining tool quantity, n=2 for tool life, n=3 for tool usage life, n=6 for tool warning life                                                                                 |

::: tip
The number of axes starts from 1 and increases according to the actual number of axes.
To use the tool life management function, the corresponding parameters on the device must be enabled. 
If the parameter address does not set `@p`, it defaults to path 1.
:::


*CNC address example*

| address         | description                                  |
| --------------- | -------------------------------------------- |
| actf            | read actual feed rate                        |
| absolute.1      | read absolute position of no.1 axis          |
| absolute.1@2    | read absolute position of no.1 axis by path2 |
| machine.3       | read machine position of no.3 axis           |
| spload.1        | read load information of no.1 spindle        |
| spmaxrpm.3      | read maximum r.p.m ratio  of no.3 spindle    |
| param.6712.0    | parts total                                  |
| param.6711.0    | parts count                                  |
| param.6750.0    | power on time                                |
| param.6753.0    | cutting on time                              |
| macro.3142.0    | Value of macro variable #3142                |
| tool.1.0        | Tool number 1                                |
| tool.1.1        | Usage count of tool number 1                 |
| tool.1.2        | Total life of tool number 1                  |
| tool.1.3        | Warning life of tool number 1                |
| diagdata.1333.3 | Data of diagnostic number 1333               |
| tlGrpinfo.1.1   | Remaining tool life of tool life group 1     |
| toolOffset.1.2  | Length wear of tool number 1                 |

## PMC Data

Address Format
`AREA ADDRESS[.BIT][.LEN][@p]`



| tag address | description                     | data type | access     |
| ----------- | ------------------------------- | --------- | ---------- |
| A           | message demand                  | all       | read/write |
| C           | counter                         | all       | read/write |
| D           | data table                      | all       | read/write |
| E           | extended relay                  | all       | read/write |
| F           | signal to CNC -> PMC            | all       | read       |
| G           | signal to PMC -> CNC            | all       | read/write |
| K           | keep relay                      | all       | read/write |
| M           | input signal from other device  | all       | read/write |
| N           | output signal from other device | all       | read/write |
| R           | internal relay                  | all       | read/write |
| T           | changeable timer                | all       | read/write |
| X           | signal to machine -> PMC        | all       | read       |
| Y           | signal to PMC -> machine        | all       | read/write |

*PMC address example*

| address | data type                                                      | descrption                                                               |
| ------- | -------------------------------------------------------------- | ------------------------------------------------------------------------ |
| A0      | uint8/int8/uint16/int16/uint32/int32/int64/uint64/float/double | PMC **message demand**，address 0                                        |
| A0.1    | bit                                                            | PMC **message demand** ，no.1 bit of address 0                           |
| A0.0    | bit                                                            | PMC **message demand** ，no.0 bit of address 0                           |
| A0.2    | string                                                         | PMC **message demand** ，address 0 starts with a string of length 2      |
| D0.2    | string                                                         | PMC **data table** ，address 0 starts with a string of length 2          |
| D0.7    | bit                                                            | PMC **data table** ，no.7 bit of address 0                               |
| G12     | uint8                                                          | PMC **signal to PMC -> CNC** ，address 12, 255-(G12)，feedrate overriden |
| G30     | uint16                                                         | PMC **signal to PMC -> CNC** ，address 30, spindle overriden             |