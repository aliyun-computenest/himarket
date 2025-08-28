This guide helps users quickly use HiMarket in the computing nest to build a privatized MCP market and help enterprises build AI mid-end.

[https://github.com/higress-group/himarket](https://github.com/higress-group/himarket)

##1. deploy HiMarket by computing nest
### HiMarket Project Structure Description
HiMarket currently involves portal-bootstrap(Java back-end), api-portal-admin (back-end front-end) and api-portal-frontend (front-end front-end) three projects, and depends on a Mysql database instance.

### One-click deployment HiMarket
[Create HiMarket AI Open Platform Task in the Computing Nest Console](https://computenest.console.aliyun.com/service/instance/create/cn-hangzhou?type=user&ServiceId=service-a9acee41142746928283&ServiceVersion=beta) can pull up the entire AI open platform environment with one click.

![](images-en/1756301233627-e4f71bd8-46cc-468c-96a2-ee359836878b_1756347255.png)

Select the target region and the instance type. Since this scheme includes three applications and a mysql database, it is recommend to use the instance configuration above 2C4G. After configuring the charging method, available area and network configuration, click OK to order and immediately create AI open platform.![](images-en/1756301119586-64abc28c-5e27-4faf-bb27-9248476b9d2a_1756347255.png)

Click OK and wait for the instance to be created.

I can see the deployment progress of the instance in my instance

![](images-en/1756301189851-348e9dd9-7f3e-4540-98f1-45c854c7b20a_1756347255.png)

After the deployment is complete, click the instance to obtain the links to access the foreground and background of the AI open platform.

### Replace MySQL instance (optional)
One-click deployment through computing nest is to deploy three projects and mysql instances through ECS. Users can quickly experience the HiMarket MCP mid-end capabilities. If users have further production requirements, they can switch Mysql to the high availability version of RDS instances and only need to replace DB_HOST configuration through environment variables in portal-bootstrap service.

##2. HiMarket background management
### Register Administrator
Visit' http:// localhost:5174 'to register an administrator account for the first time.

![](images-en/1756119821010-918f90e0-8975-4e4b-9c49-fed73bd5da3e_1756347255.png)

### Import an AI Gateway instance
Select [Instance Management]-[Gateway Instance]-[Import Gateway Instance]-[AI Gateway], prepare the AK/SK of the sub-account, select the instance corresponding to the region, and import the AI gateway instance, taking xxx as an example.

![](images-en/1756120367063-e2c9d5e5-9bad-4193-a681-c59145839c54_1756347255.png)

It involves the application of sub-account AK/SK. In order to avoid the risk of leakage, you need to select the minimum permission: for details, see: [Minimum RAM permission required for integrated AI/API gateway](https://aliyuque.antfin.com/ah5vgb/kg7h1z/oc8wb6f5mpa9xssd?singleDoc#)

### Create Portal Portal
Select Portal-Create Portal to create a portal named himarket-demo.

![](images-en/1756120440508-7d2960b3-9439-43b9-91e6-de371e5ee76e_1756347255.png)

Click the portal card to enter the portal configuration, and keep the default options for other configurations. In [Setting]-[Domain Name Management]-[Bind Domain Name], bind a localhost domain name for development self-test. Other menus can be used without attention in the quick start. Here is a brief introduction to their functions:

Published API Products. Manage API Product published in the portal.
Developers. Manage the Developer of the portal and Developer associated Consumer.
Settings.
-Configure the basic information of the portal.
-Control whether the registration approval of the Developer in the portal is automatically approved, and whether the API Product subscription is automatically approved.
-Portal supports three-way login. Supports standard OIDC configurations, such as Aliyun, Google, and Github.

### Create an API Product
Select [API Products]-[Create API Product] to create a demo-api API Product.

![](images-en/1756120817284-7d572a58-15b2-41ca-8798-f8514dd48fec_1756347255.png)

The initial status of the API Product is "to be configured", and operations such as Link API and publishing to the portal are required.

### Associated API
![](images-en/1756135284300-92fbfc3d-4927-42d2-91eb-ef586a2ae083_1756347255.png)

The MCP service associated with a gateway. The data source comes from the MCP service management of the AI gateway. API Config also automatically synchronizes the configuration in the Higress.

### Usage Guide
![](images-en/1756135522434-60d162f9-3007-487d-a19b-36fc323e9bd3_1756347255.png)

You can edit the customized document information in the user guide.

### Publish to Portal
After the API Product is ready, you can choose to publish to the specified portal.

![](images-en/1756135616759-69d1a672-cadc-40af-b329-0e1c712569bc_1756347255.png)

At this point, a Higress MCP Server has been successfully published to the portal.

##3. HiMarket Portal
The portal will have a domain name assigned by default, but the domain name resolution needs to be completed by the user. For example, if the portal-68ac4564bdb292ee9261ff4a.api.portal.local domain name is automatically assigned, it needs to be resolved to the IP address corresponding to the api-portal-frontend.

Since localhost domain name has just been additionally configured for the test portal, the front desk can also be accessed directly through localhost:5173.

### Registered Developer Developer
![](images-en/1756136341614-78e70a99-6165-4ef0-9c7d-839538f32651_1756347255.png)

Since automatic approval was not turned on in the portal settings before, you need to wait for the administrator to approve the developer in the background after registering the account. After the approval is passed, you can use the registered account to log in at the foreground.

![](images-en/1756136953577-e7259989-49b7-4fb3-9ac3-023453fcb303_1756347255.png)

Visit the MCP portal to see the MCP Server just released

![](images-en/1756137076017-354e06c7-62e2-458b-bc08-c958f3b0d00d_1756347255.png) ![](images-en/1756137117244-4ffdaabe-3459-43df-a79f-a3a74fcb8641_1756347255.png)

### Create a Consumer consumer
In the design of AI open platform, consumers Developer represent general user identities, and users need to hold corresponding credentials to apply for subscription to API Product. The concept of credentials is called Consumer consumers in AI open platform, and Developer and Consumer are one-to-many related.

![](images-en/1756137408652-ececcf32-15a6-4b8d-a76a-6a8c6795e49a_1756347255.png)

After creating a consumer, you can apply for an API Product subscription.

![](images-en/1756137466611-a90abc4b-7f39-4427-9443-671ddd24b5de_1756347255.png)

In the default configuration of the portal, the subscription approval is disabled by default, that is, the developer will automatically approve the subscription after applying.

### Initiate Call
You can call the MCP server by using the consumer's credentials and configuring the connection address of the MCP server in the portal.

