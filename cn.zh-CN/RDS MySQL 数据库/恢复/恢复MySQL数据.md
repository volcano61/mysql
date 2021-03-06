# 恢复MySQL数据

如果拥有RDS MySQL实例的数据备份，可以通过备份恢复的方式实现数据修复。

原实例需要满足如下条件：

-   运行中且没有被锁定。
-   当前没有迁移任务。
-   如果要按时间点进行恢复，需要确保日志备份已开启。
-   若要按备份集恢复，则原实例必须至少有一个物理备份。

其他引擎恢复数据请参见：

-   [恢复SQL Server数据](/cn.zh-CN/RDS SQL Server 数据库/恢复/恢复SQL Server数据.md)
-   [恢复PostgreSQL数据](/cn.zh-CN/RDS PostgreSQL 数据库/恢复/恢复PostgreSQL数据.md)
-   [恢复PPAS数据](/cn.zh-CN/RDS PPAS 数据库/恢复/恢复PPAS数据.md)
-   [恢复MariaDB数据](/cn.zh-CN/RDS MariaDB TX 数据库/恢复/恢复MariaDB数据.md)

## 背景信息

您可以通过以下方式恢复RDS MySQL实例的数据：

-   方式一：恢复到一个新实例，经过验证后，再将数据迁回原实例，此功能原名为克隆实例。本文介绍这种方式。
-   方式二：恢复单库和单表的数据到原实例或新实例。详情请参见[MySQL单库单表恢复](/cn.zh-CN/RDS MySQL 数据库/恢复/MySQL单库单表恢复.md)。
-   方式三：跨地域恢复数据到新实例或已有实例。详情请参见[跨地域恢复数据](/cn.zh-CN/RDS MySQL 数据库/恢复/跨地域恢复数据.md)。

**说明：** 恢复到自建数据库请参见[RDS MySQL物理备份文件恢复到自建数据库](/cn.zh-CN/RDS MySQL 数据库/恢复/RDS MySQL物理备份文件恢复到自建数据库.md)或[RDS MySQL逻辑备份文件恢复到自建数据库](/cn.zh-CN/RDS MySQL 数据库/恢复/RDS MySQL逻辑备份文件恢复到自建数据库.md)。

## 注意事项

-   新实例的白名单设置、备份设置、参数设置和当前实例保持一致。
-   新实例内的数据信息与备份文件或时间点当时的信息一致。
-   新实例带有所使用备份文件或时间点当时的账号信息。

## 费用

由于数据是恢复到新实例上，因此需要收取新实例费用。详情请参见[价格、收费项与计费方式](/cn.zh-CN/云数据库 RDS 价格/价格、收费项与计费方式.md)。

**说明：** 通过数据传输DTS将新实例的数据迁移回原实例时，不收取结构迁移和全量迁移的费用。

## 恢复数据到新实例

1.  登录[RDS管理控制台](https://rds.console.aliyun.com/)。

2.  在左侧单击**实例列表**，然后在上方选择实例所在地域。

    ![选择地域](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3074469951/p36543.png)

3.  找到目标实例，单击实例ID。

4.  在左侧导航栏选择**备份恢复**。

5.  单击**数据库恢复（原克隆实例）**。

6.  设置如下参数。

    |类别|说明|
    |--|--|
    |**计费方式**|    -   **包年包月**：属于预付费，即在新建实例时需要支付费用。适合长期需求，价格比按量付费更实惠，且购买时长越长，折扣越多。
    -   **按量付费**：属于后付费，即按小时扣费。适合短期需求，用完可立即释放实例，节省费用。 |
    |**还原方式**|    -   **按时间点**：可以设置为日志备份保留时间内的任意时间点（任意一秒）。如要查看或修改日志备份保留时间，请参见[备份MySQL数据](/cn.zh-CN/RDS MySQL 数据库/备份/备份MySQL数据.md)。
    -   **按备份集**：恢复所选备份集内的数据。备份集只能为物理备份，暂不支持逻辑备份。
**说明：** 只有开启了日志备份，才会显示**按时间点**。 |
    |**可用区**|可用区是地域中的一个独立物理区域，**主节点可用区**指主实例所在可用区，**备节点可用区**指备实例所在可用区。

您可以设置实例为**单可用区部署**或**多可用区部署**：

    -   **单可用区部署**：**主节点可用区**和**备节点可用区**都处于相同可用区。
    -   **多可用区部署**（推荐）：**主节点可用区**和**备节点可用区**处于不同可用区，能提供可用区级别的容灾。您只需要选择**主节点可用区**，系统会自动选择**备节点可用区**。
**说明：**

    -   实例创建后，您可以在实例的**服务可用性**页面查看主备节点信息。
    -   基础版实例只有一个节点，只能部署在一个可用区内。
![可用区](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1677559951/p87361.png) |
    |**实例规格**|    -   **通用规格（入门级）**：通用型的实例规格，独享被分配的内存和I/O资源，与同一服务器上的其他通用型实例共享CPU和存储资源。
    -   **独享规格（企业级）**：独享或独占型的实例规格。独享型指独享被分配的CPU、内存、存储和I/O资源。独占型是独享型的顶配，独占整台服务器的CPU、内存、存储和I/O资源。
    -   **专属规格**：完全独享虚拟主机或物理主机资源，开放主机权限，可直接在主机上按需分配多个数据库实例。更多信息，请参见[添加主机]()。
**说明：** 每种规格都有对应的CPU核数、内存、最大连接数和最大IOPS。详情请参见[主实例规格列表](/cn.zh-CN/云数据库 RDS 简介/产品规格/主实例规格列表.md)。 |
    |**存储空间**|存储空间包括数据空间、系统文件空间、Binlog文件空间和事务文件空间。调整存储空间时最小单位为5GB。 **说明：**

    -   部分本地SSD盘的存储空间大小与实例规格绑定，ESSD/SSD云盘不受此限制。详情请参见[主实例规格列表](/cn.zh-CN/云数据库 RDS 简介/产品规格/主实例规格列表.md)。
    -   如果存储类型为云盘，默认会勾选**存储空间自动扩展**，当存储空间过小时，会自动扩展存储空间，避免实例锁定。 |

7.  单击**下一步：实例配置**。

8.  设置如下参数。

    |类别|说明|
    |--|--|
    |**网络类型**|    -   **经典网络**：传统的网络类型。
    -   **专有网络**（推荐）：也称为VPC（Virtual Private Cloud）。VPC是一种隔离的网络环境，安全性和性能均高于传统的经典网络。选择专有网络时您需要选择对应的**VPC**和**主节点交换机**。
**说明：** 请确保RDS实例与需要连接的ECS实例网络类型一致（如果选择专有网络，还需要保证VPC一致），否则它们无法通过内网互通。 |

9.  单击**下一步：确认订单**。

10. 确认**参数配置**，选择**购买量**和**购买时长**（仅包年包月实例），勾选服务协议，单击**去支付**完成支付。

    **说明：** 对于包年包月实例，建议您勾选**到期自动续费**，可以免去您定期手动续费的烦恼，且不会因忘记续费而导致业务中断。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6867559951/p52773.png)


## 登录到新实例并验证数据

关于登录实例的操作，详情请参见[连接实例](/cn.zh-CN/RDS MySQL 数据库/快速入门/连接MySQL实例.md)。

## 迁移数据到原实例

确认新实例的数据之后，您可以将需要的数据从新实例迁移回原实例。详情请参见[RDS实例间的数据迁移](https://help.aliyun.com/document_detail/26626.html)。

**说明：** 数据迁移是指将一个实例（称为源实例）的数据复制到另一个实例（称为目标实例），迁移操作不会对源实例造成影响。

## 常见问题

-   不小心删除了数据库，如何恢复？

    您可以进行单库恢复，详情请参见[MySQL单库单表恢复](/cn.zh-CN/RDS MySQL 数据库/恢复/MySQL单库单表恢复.md)。对于不支持单库单表恢复的实例，您可以参见本文，将数据全量恢复到新实例上，经过验证后，再将数据迁回原实例。

-   没有数据备份可以按时间点恢复吗？

    不可以。因为按时间点恢复是先将所选时间点前的一个全量数据备份恢复到实例，然后根据Binlog增量恢复数据到所选时间点。

-   为什么恢复时无法选择主节点交换机？

    可能因为您在前一步（基础配置）选择的可用区内没有交换机，所以在当前步骤（网络和资源组）无法选择主节点交换机。您可以单击**到控制台创建**跳转到专有网络控制台，在可用区内创建交换机，就可以选择主节点交换机了。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2313729951/p70932.png)


