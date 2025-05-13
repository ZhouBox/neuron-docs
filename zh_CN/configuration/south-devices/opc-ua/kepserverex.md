# KEPServerEX 连接示例

KEPServerEX 是由 Kepware 公司开发的一款工业自动化设备与应用间的通信连接服务软件，它是一款在工业控制中常见的数据采集服务软件，提供了各种类型的驱动，使得工业设备可以与各类控制硬件和软件进行高效、可靠的通信。KEPServerEX 利用 OPC（自动化行业的互操作性标准）和以 IT 为中心的通信协议（如 SNMP、ODBC 和 Web 服务）为用户提供工业数据的单一来源。

本节将演示如何通过 Neuron OPC UA 插件连接 KEPServerEX。

## 连接 OPC UA Server（用户名/密码）

1. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **设置** -> **KEPServerEX 设置** -> **用户管理器**，在 **Administrators** 组下新建用户，设置用户名/密码。

   ![kepware-1](./assets/kepware-1.jpg)

2. 双击系统托盘中的 KEPServerEX 图标，在主界面中打开 **项目** -> **属性编辑器** -> **OPC UA**， **允许匿名登录** 设置为 **否**。

   ![kepware-2](./assets/kepware-2.jpg)

3. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **OPC UA 配置** -> **服务器端点**，双击端点条目，勾选所有安全策略。  

   ![kepware-3](./assets/kepware-3.jpg)

4. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **重新初始化**。

## 配置 Neuron

1. 通过 UaExpert 软件查看 KepServerEx 测点信息， 参考 [UaExpert](./uaexpert.md)。

   ![kepware-5](./assets/kepware-5.jpg)

2. Neuron 新增南向 OPC UA 设备，打开 **设备配置**，填写目标 Server 的 端点 URL，填写用户名/密码，无需添加证书/密钥，启动设备连接；

3. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **OPC UA 配置** -> **受信任的客户端**，将 NeuronClient 证书设置为信任。
   ![kepware-4](./assets/kepware-4.jpg)

  

3. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **重新初始化**；

## 连接 OPC UA Server（证书/密钥 + 用户名/密码）

1. 按照上文设置用户名/密码；

2. 参考[连接策略](./policy.md)生成或转换证书/密钥；

3. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **OPC UA 配置** -> **受信任的客户端**，将 DER 格式的客户端证书导入列表；

4. 右键点击系统托盘中的 KEPServerEX 图标，在菜单中选择 **重新初始化**。

## 配置 Neuron

1. 通过 UaExpert 软件查看 KepServerEx 测点信息， 参考 [UaExpert](./uaexpert.md)。

   ![kepware-5](./assets/kepware-5.jpg)


2. Neuron 新增南向 OPC UA 设备，打开 **设备配置**，填写目标 Server 的 **端点 URL**，填写用户名/密码，添加证书/密钥。

3. 可以修改**更新模式**为 Subscribe 或 Read&Subscribe，以 OPC UA 订阅方式获取数据。

4. 启动设备连接；

5. 根据测点信息添加 **Groups** 和 **Tags**。

## 测试点位

| 名称     | 地址                                       | 属性       | 类型   |
| -------- | ------------------------------------------ | ---------- | ------ |
| Boolean1 | 2!数据类型示例.16 位设备.R 寄存器.Boolean1 | Read Write | BOOL   |
| DWord1   | 2!数据类型示例.16 位设备.R 寄存器.DWord1   | Read Write | UINT32 |
| Double1  | 2!数据类型示例.16 位设备.R 寄存器.Double1  | Read Write | DOUBLE |
| Float1   | 2!数据类型示例.16 位设备.R 寄存器.Float1   | Read Write | FLOAT  |
| LLong1   | 2!数据类型示例.16 位设备.R 寄存器.LLong1   | Read Write | INT64  |
| Long1    | 2!数据类型示例.16 位设备.R 寄存器.Long1    | Read Write | INT32  |
| QWord1   | 2!数据类型示例.16 位设备.R 寄存器.QWord1   | Read Write | UINT64 |
| Short1   | 2!数据类型示例.16 位设备.R 寄存器.Short1   | Read Write | INT16  |
| Word1    | 2!数据类型示例.16 位设备.R 寄存器.Word1    | Read Write | UINT16 |
| String1  | 2!数据类型示例.16 位设备.S 寄存器.String1  | Read Write | STRING |

