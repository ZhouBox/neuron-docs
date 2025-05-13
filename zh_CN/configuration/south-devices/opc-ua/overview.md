# OPC UA

OPC UA 是一种面向工业自动化的机器到机器通信协议，由 OPC 基金会开发维护。OPC UA 提供一种标准化的方式， 使不同的设备和系统能够互相通信。

Neuron OPC UA 插件可作为客户端访问 KEPServerEX、Industrial Gateway OPC Server、Prosys Simulation Server、Ignition 等 OPC UA 服务器，也可以直接访问硬件设备的内置 OPC UA Server，如西门子 S7-1200 型 PLC 的内置 Server、 欧姆龙 NJ 系列 PLC 的内置 Server 等。

::: tip

OPC UA  服务端目前支持匿名方式、用户名/密码方式、证书/密钥 + 匿名方式和证书/密钥 + 用户名/密码方式等方式访问，具体可参考 [OPC UA 连接策略](./policy.md)。

:::

## 添加插件

在 **配置 -> 南向设备**，点击**添加设备**来创建设备节点，输入插件名称，插件类型选择 **OPC UA** 启用插件。

## 设备配置

点击插件卡片或插件列，进入**设备配置**页。配置 Neuron 与设备建立连接所需的参数，下表为 OPC UA 相关配置项。

| 参数         | 说明                                                            |
| ------------ | --------------------------------------------------------------- |
| **端点 URL** | 目标 OPC UA Server 的 URL，默认值是 `opc.tcp://127.0.0.1:4840/` |
| **用户名**   | 连接目标 OPC UA Server 使用的用户名                             |
| **密码**     | 连接目标 OPC UA Server 使用的密码                               |
| **证书**     | DER 格式的客户端证书                                            |
| **密钥**     | DER 格式的客户端密钥                                            |
| **安全策略**  | 设置 OPC UA 连接的安全策略，None/Sign/Sign&Encrypt，默认为 None |
| **更新模式**  | 设置 OPC UA 采集数据的方式，Read/Subscribe/Read&Subscribe，默认为 Read |
| **发布间隔**  | 设置 OPC UA 以 Subscribe/Read&Subscribe 模式采集数据时，两次发布之间的最小间隔 |

## 更新模式
**Read** 模式，数据更新方式与之前版本的方式一致，采用 OPC UA 标准的读取接口获取 Server 端数据，这也是默认的数据更新方式。

**Subscribe** 模式，新增的数据更新方式，采用 OPC UA 标准的订阅接口获取 Server 数据，订阅成功会批量更新一次数据，后续只有 Server 端数据变化才会更新。

**Read&Subscribe** 模式，新增的数据更新方式，同时采用读取接口和订阅接口获取 Server 端数据。

**发布间隔**定义了当**更新模式**设置为 Subscribe 或 Read&Subscribe 模式时， OPCUA 服务器向 NeuronEX 循环发送点位更新值的最快速率。如果该参数设置为0，使用 OPCUA 服务器支持的最快发布间隔。该间隔以毫秒为单位表示。默值值为 500ms，表示 OPC UA 服务器发送给 NeuronEX 最快的点位更新速率为 500ms。

::: tip
**更新模式**在南向节点的全局参数中设置，表示该南向节点以 OPC UA 的订阅方式获得数据。而 NeuronEX 点位设置中的 Subscribe 属性表示这个点位的数值发生变化时立即触发北向应用的上送报告。同时设置二者便可以获得实时的数据变化报告。
:::

## 设置组和点位

完成插件的添加和配置后，要建立设备与 Neuron 之间的通信，首先为南向驱动程序添加组和点位。

完成设备配置后，在**南向设备**页，点击设备卡片/设备列进入**组列表**页。点击**创建**来创建组，设定组名称以及采集间隔。完成组的创建后，点击组名称进入**点位列表**页，添加需要采集的设备点位，包括点位地址，点位属性，数据类型等。

公共配置项部分可参考[连接南向设备](../south-devices.md)，本页将介绍支持的数据类型和地址格式部分。

::: tip

可以使用 UaExpert 软件协助查看所需点位的命名空间索引和节点 ID 信息，参考 [UaExpert](./uaexpert.md)。

- 关于命名空间索引和节点 ID 的解释，请参考 OPC UA 标准。

- Neuron 设置的数据类型必须与 OPC UA 数据类型相匹配。

:::

### 数据类型

* INT8（用于表示 OPC UA SBYTE 类型）
* INT16
* INT32
* INT64
* UINT8（用于表示 OPC UA BYTE 类型）
* UINT16
* UINT32（同时用于表示 OPC UA DATETIME 类型）
* UINT64
* FLOAT
* DOUBLE
* BOOL
* BIT
* STRING
* ARRAY_CHAR
* ARRAY_INT8
* ARRAY_UINT8
* ARRAY_INT16
* ARRAY_UINT16
* ARRAY_INT32
* ARRAY_UINT32
* ARRAY_INT64
* ARRAY_UINT64
* ARRAY_FLOAT
* ARRAY_DOUBLE
* ARRAY_BOOL
* JSON

#### 数据类型转换

| OPC UA 数据类型         | Neuron 数据类型            |
| ----------------------- | -------------------------- |
| SByte                   | INT8                       |
| Int16                   | INT16                      |
| Int32                   | INT32                      |
| Int64                   | INT64                      |
| Byte                    | UINT8                      |
| UInt16                  | UINT16                     |
| UInt32                  | UINT32                     |
| UInt64                  | UINT64                     |
| Float                   | FLOAT                      |
| Double                  | DOUBLE                     |
| Boolean                 | BOOL (采集值为true或false) |
| Boolean                 | BIT (采集值为0或1)         |
| String                  | STRING                     |
| Datetime                | UINT32                     |
| LocalizedText（只读）   | STRING                     |
| SByte Array             | ARRAY_INT8                 |
| Int16 Array             | ARRAY_INT16                |
| Int32 Array             | ARRAY_INT32                |
| Int64 Array             | ARRAY_INT64                |
| Byte  Array             | ARRAY_UINT8, ARRAY_CHAR    |
| UInt16 Array            | ARRAY_UINT16               |
| UInt32 Array            | ARRAY_UINT32               |
| UInt64 Array            | ARRAY_UINT64               |
| Float  Array            | ARRAY_FLOAT                |
| Double Array            | ARRAY_DOUBLE               |
| Boolean Array           | ARRAY_BOOL                 |
| OPCUA Exentision Object | JSON                       |

ARRAY_CHAR 展示和写入形式为字符串。
JSON 展示和写入形式为 JSON 字符串。

### 地址格式

> NS[x,y,z]!NODEID

**NS** 名字空间索引。

**[x,y,z]** 数组索引，当数据类型为数组时，可以使用索引获取到特定位置的数值，维度从 0 开始计数，维度不超过 2。 x 表示一维索引，y 表示二维索引，z 表示三维索引。

**NODEID** 节点 ID，可以设置为数字形式、字符串形式和 GUID 形式。

### 地址示例

| 地址                                   | 数据类型 | 说明                                                                                                                       |
| -------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| 0!2258                                 | UINT32   | 使用数字类型的 NODEID，获取 OPC UA 服务器的时间戳；NS 为0，NODEID 为2258                                                   |
| 2!Device1.Module1.Tag1                 | INT8     | 使用字符串类型的 NODEID，获取类型为 SBYTE 的数据点；NS 为2，NODEID 为 Device1.Module1.Tag1                                 |
| 0!c496578a-0dfe-4b8f-870a-745238c6ae00 | BOOL     | 使用 GUID 类型的 NODEID，获取类型为 BOOL 的数据点；NS 为0，NODEID 为c496578a-0dfe-4b8f-870a-745238c6ae00                   |
| 1[2]!array1d[string]                   | STRING   | 访问 STRING 数组的第 3 个元素；NS 为1，NODEID 为 array1d[string], 一维索引为 2                                             |
| 1[2,3]!array2d[int]                    | INT32    | 访问 INT32 数组的第 3 行，第 4 列的元素；NS 为1，NODEID 为 array2d[int], 一维索引为 2，二维索引为 3                        |
| 1[2,3,4]!array3d[float]                | FLOAT    | 访问 FLOAT 数组的第 3 行，第 4 列，第 5 个元素；NS 为1，NODEID 为 array3d[float], 一维索引为 2，二维索引为 3，三维索引为 4 |
| 0!3D.Point                             | JSON     | 获取一个关于 3D 位置的扩展对象，对象包含 x，y，z 三个成员元素                                                              |

## 应用场景
本节还提供以下应用场景示例，方便您快速上手：

- [Siemens S7-1200](s71200.md)
- [KEPServerEX](kepserverex.md)
- [Industrial Gateway OPC Server](igs.md)
- [Ignition](ignition.md)
- [Prosys Simulation Server](prosys)

## 数据监控
完成点位的配置后，您可点击 **监控** -> **数据监控**查看设备信息以及反控设备，具体可参考[数据监控](../../../admin/monitoring.md)。

