# Apply for or release a public endpoint on an ApsaraDB RDS for SQL Server instance

ApsaraDB RDS for SQL Server supports two types of endpoints: internal endpoints and public endpoints. By default, you are provided with an internal endpoint that is used to connect to your RDS instance. If you want to connect to your RDS instance over the Internet, you must apply for a public endpoint.

## Internal and public endpoints

|Endpoint type|Description|
|-------------|-----------|
|Internal endpoint|-   An internal endpoint is provided by default. You do not need to apply for this endpoint. In addition, you cannot release this endpoint. However, you can change the network type of your RDS instance.
-   If an Elastic Compute Service \(ECS\) instance resides in the same region and has the same network type as your RDS instance, these instances can communicate over an internal network. If your application is deployed on such an ECS instance, you do not need to apply for a public endpoint. For more information, see [Change the network type of an ApsaraDB RDS for SQL Server instance](/intl.en-US/RDS SQL Server Database/Database connection/Change the network type of an ApsaraDB RDS for SQL Server instance.md).
-   For security and performance purposes, we recommend that you connect to your RDS instance by using the internal endpoint. |
|Public endpoint|-   You must manually apply for a public endpoint. You can release this endpoint if it is no longer required.
-   If you cannot connect to your RDS instance by using the internal endpoint, you must apply for a public endpoint. This includes the following scenarios:
    -   Connect to your RDS instance from an ECS instance that resides in a different region or has a different network type from your RDS instance. For more information, see [Change the network type of an ApsaraDB RDS for SQL Server instance](/intl.en-US/RDS SQL Server Database/Database connection/Change the network type of an ApsaraDB RDS for SQL Server instance.md).
    -   Connect to your RDS instance from a device outside Alibaba Cloud.

**Note:**

-   You are not charged for the public endpoint or the traffic that is consumed.
-   If you connect to the RDS instance by using the public endpoint, the security of the RDS instance is compromised. Proceed with caution.
-   To expedite transmission and improve security, we recommend that you migrate your application to an ECS instance that resides in the same region and has the same network type as the RDS instance. This allows you to connect to the RDS instance by using the internal endpoint. |

## Procedure

1.  Log on to the [ApsaraDB for RDS console](https://rds.console.aliyun.com/).

2.  In the left-side navigation pane, click **Instances**. In the top navigation bar, select the region where your RDS instance resides.

    ![Select a region](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8651559951/p36543.png)

3.  Find your RDS instance and click its ID.

4.  In the left-side navigation pane, click **Database Connection**.

5.  Apply for or release an endpoint:

    -   If you have not applied for a public endpoint, you can click **Apply for Public Endpoint**.
    -   If you have applied for a public endpoint, you can click **Release Public Endpoint**.
    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6150359951/p11667.png)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6150359951/p3993.png)

6.  In the message that appears, click **Confirm**.


## Related operations

|API|Description|
|---|-----------|
|[Apply for public endpoint](/intl.en-US/API Reference/Database Connection/Apply for public endpoint.md)|Applies for a public endpoint for an ApsaraDB for RDS instance.|
|[t8139.md\#](/intl.en-US/API Reference/Database Connection/Release a public endpoint.md)|Releases the public endpoint of an ApsaraDB for RDS instance.|

