This guide helps users quickly use HiMarket in the computing nest to build a privatized MCP market and help enterprises build AI mid-end.

Github Address:[https://github.com/higress-group/himarket](https://github.com/higress-group/himarket)

##1. deploy HiMarket by computing nest
### HiMarket Project Structure Description
HiMarket currently involves portal-bootstrap(Java back-end), api-portal-admin (back-end front-end) and api-portal-frontend (front-end front-end) three projects, and depends on a Mysql database instance.

### One-click deployment HiMarket
[Create HiMarket AI Open Platform Task in the Computing Nest Console](https://computenest.console.aliyun.com/service/instance/create/cn-hangzhou?type=user&ServiceId=service-a9acee41142746928283&ServiceVersion=beta) can pull up the entire AI open platform environment with one click.



Select the target region and the instance type. Since this scheme includes three applications and a mysql database, it is recommend to use the instance configuration above 2C4G. After configuring the charging method, available area and network configuration, click OK to order and immediately create AI open platform.![](images-en/1756301119586-64abc28c-5e27-4faf-bb27-9248476b9d2a_1756361939.png)

Click OK and wait for the instance to be created.

I can see the deployment progress of the instance in my instance

![](images-en/1756301189851-348e9dd9-7f3e-4540-98f1-45c854c7b20a_1756361939.png)

After the deployment is complete, click the instance to obtain the links to access the foreground and background of the AI open platform.

![](images-en/1756352169584-d5c9cdb3-c411-465b-905c-cd268d8e7fad_1756361939.png)

### Replace MySQL instance (optional)
One-click deployment through computing nest is to deploy three projects and mysql instances through ECS. Users can quickly experience the HiMarket MCP mid-end capabilities. If users have further production requirements, they can switch Mysql to the high availability version of RDS instances and only need to replace DB_HOST configuration through environment variables in portal-bootstrap service.

##2. HiMarket background management
### Register Administrator
Visit the administrator access page' http://47.xx.xx.xx:5174 'to register an administrator account for the first time.

![](images-en/1756119821010-918f90e0-8975-4e4b-9c49-fed73bd5da3e_1756361939.png)

### Import an AI Gateway instance
Select [Instance Management]-[Gateway Instance]-[Import Gateway Instance]-[AI Gateway], prepare the AK/SK of the sub-account, select the instance corresponding to the region, and import the AI gateway instance. Take gw-xxx as an example.

![](images-en/1756352419273-dce17653-6df0-4950-8a51-b4f060d163c7_1756361939.png)

![](images-en/1756352393408-98066e45-e7a1-43d0-bbe9-a8c83129677b_1756361939.png)

It involves the application of the sub-account AK/SK. In order to avoid the risk of leakage, you need to create a sub-account and select the minimum permissions. The relevant permissions are as follows:

APIG read-only permission: APIGReadOnlyAccess
<font style = "color:rgb (51,51, 51);background-color:rgb(250, 250, 250);"> Metering/Logging/SLS read-only permissions:</font>AliyunLogReadOnlyAccess
APIG Consumer/Policy/Plug-in Operation Related Write Permission

'''json
{
"Version": "1 ",
"Statement ": [
{
"Effect": "Allow ",
"Action ": [
"apig:*Consumer* ",
"apig:*Policy* ",
"apig:*Plugin*"
],
"Resource": "*"
}
]
}
'''

When using AK/SK, pay attention to the risk of AK/SK leakage.

### Create Portal Portal
Select Portal-Create Portal to create a portal named himarket-demo.

![](images-en/1756352736170-4cc49554-c008-4dd5-9732-7911187418c4_1756361939.png)

Click the portal card to enter the portal configuration, and keep the default options for other configurations. In [Setting]-[Domain Name Management]-[Bind Domain Name], bind a localhost domain name for development self-test. Other menus can be used without attention in the quick start. Here is a brief introduction to their functions:

Published API Products. Manage API Product published in the portal.
Developers. Manage the Developer of the portal and Developer associated Consumer.
Settings.
-Configure the basic information of the portal.
-Control whether the registration approval of the Developer in the portal is automatically approved, and whether the API Product subscription is automatically approved.
-Portal supports three-way login. Supports standard OIDC configurations, such as Aliyun, Google, and Github.

### Create an API Product
Select [API Products]-[Create API Product] to create a demo-api API Product.

![](images-en/1756120817284-7d572a58-15b2-41ca-8798-f8514dd48fec_1756361939.png)

The initial status of the API Product is "to be configured", and operations such as Link API and publishing to the portal are required.

### Associated API
![](images-en/1756352703144-498a0db7-bf9e-44fb-b408-b4c4c8b6cb37_1756361939.png)

The MCP service associated with a gateway. The data source comes from the MCP service management of the AI gateway. API Config also automatically synchronizes the configuration in the AI gateway.

### Usage Guide
![](images-en/1756135522434-60d162f9-3007-487d-a19b-36fc323e9bd3_1756361939.png)

You can edit the customized document information in the user guide.

### Publish to Portal
After the API Product is ready, you can choose to publish to the specified portal.

![](images-en/1756135616759-69d1a672-cadc-40af-b329-0e1c712569bc_1756361939.png)

At this point, a Higress MCP Server has been successfully published to the portal.

##3. HiMarket Portal
The portal will have a domain name assigned by default, but the domain name resolution needs to be completed by the user. For example, if the portal-68ac4564bdb292ee9261ff4a.api.portal.local domain name is automatically assigned, it needs to be resolved to the IP address corresponding to the api-portal-frontend (I .e. **<font style =" color:rgb (51,51, 51) ! important;"> user use page </font>* * address IP'47.xx.xx.xx'), the administrator can manage the domain name of the current foreground Portal in the background [Portal details]-[settings]-[domain name management].

### Registered Developer Developer
![](images-en/1756136341614-78e70a99-6165-4ef0-9c7d-839538f32651_1756361939.png)

Since automatic approval was not turned on in the portal settings before, you need to wait for the administrator to approve the developer in the background after registering the account. After the approval is passed, you can use the registered account to log in at the foreground.

![](images-en/1756136953577-e7259989-49b7-4fb3-9ac3-023453fcb303_1756361939.png)

Visit the MCP portal to see the MCP Server just released

![](images-en/1756137076017-354e06c7-62e2-458b-bc08-c958f3b0d00d_1756361939.png) ![](images-en/1756137117244-4ffdaabe-3459-43df-a79f-a3a74fcb8641_1756361939.png)

### Create a Consumer consumer
In the design of AI open platform, consumers Developer represent general user identities, and users need to hold corresponding credentials to apply for subscription to API Product. The concept of credentials is called Consumer consumers in AI open platform, and Developer and Consumer are one-to-many related.

![](images-en/1756137408652-ececcf32-15a6-4b8d-a76a-6a8c6795e49a_1756361939.png)

After creating a consumer, you can apply for an API Product subscription.

![](images-en/1756137466611-a90abc4b-7f39-4427-9443-671ddd24b5de_1756361939.png)

In the default configuration of the portal, the subscription approval is disabled by default, that is, the developer will automatically approve the subscription after applying.

### Initiate Call
You can call the MCP server by using the consumer's credentials and configuring the connection address of the MCP server in the portal.

