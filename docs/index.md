本指南帮助用户快速在计算巢使用HiMarket来构建私有化MCP市场，助力企业构建 AI 中台。

[https://github.com/higress-group/himarket](https://github.com/higress-group/himarket)

## 一、通过计算巢一键部署 HiMarket
### HiMarket项目结构说明
HiMarket 目前涉及到portal-bootstrap（Java 后端）、api-portal-admin（后台前端） 和 api-portal-frontend （前台前端）三个项目，并且依赖一个Mysql数据库实例。

### 一键部署HiMarket
通过[计算巢控制台创建HiMarket AI开放平台任务](https://computenest.console.aliyun.com/service/instance/create/cn-hangzhou?type=user&ServiceId=service-a9acee41142746928283&ServiceVersion=beta)可以一键拉起整套AI开放平台环境。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/198743/1756301233627-e4f71bd8-46cc-468c-96a2-ee359836878b.png)

选择目标地域，选择实例类型，由于本方案包含了三个应用以及一个mysql数据库，推荐使用2C4G以上实例配置；配置计费方式、可用区以及网络配置后，点击确定订单，并且立即创建AI开放平台。![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/198743/1756301119586-64abc28c-5e27-4faf-bb27-9248476b9d2a.png)

点击确定后，等待实例创建完成。

在我的实例中可以看到实例的部署进度

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/198743/1756301189851-348e9dd9-7f3e-4540-98f1-45c854c7b20a.png)

部署完成后，点击实例即可获取AI开放平台的前台与后台的访问链接。

### 替换Mysql实例（可选）
通过计算巢一键部署是通过ECS方式部署三个项目以及mysql实例，用户可以快速体验HiMarket的MCP中台能力，如果用户有进一步生产使用的诉求，可以将 Mysql 切换至 RDS 实例高可用版本，并且只需要在 portal-bootstrap 服务中通过环境变量方式替换 DB_HOST 配置。

## 二、HiMarket 后台管理
### 注册管理员
访问 `http://localhost:5174`，首次访问注册一个管理员账号。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756119821010-918f90e0-8975-4e4b-9c49-fed73bd5da3e.png)

### 导入 AI 网关实例
选择【实例管理】-【网关实例】-【导入网关实例】-【AI 网关】，需要准备好子账号的AK/SK，然后选择对应Region的实例，并导入 AI 网关实例，以xxx为例。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756120367063-e2c9d5e5-9bad-4193-a681-c59145839c54.png)

其中涉及到子账号AK/SK的申请，为了避免泄漏风险，需要选择最小权限：详见：[集成AI/API网关所需的最小RAM权限](https://aliyuque.antfin.com/ah5vgb/kg7h1z/oc8wb6f5mpa9xssd?singleDoc#)

### 创建 Portal 门户
选择【Portal】-【创建 Portal】，创建一个名为 himarket-demo 的门户。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756120440508-7d2960b3-9439-43b9-91e6-de371e5ee76e.png)

点击门户卡片，进入门户配置，其他配置保留默认选项即可，在 【Setting】-【域名管理】-【绑定域名】中，绑定一个 localhost 域名，用于开发自测。其他菜单在快速入门中可以先不用关注，这里简单介绍下他们的功能：

+ Published API Products。管理门户中发布的 API Product。
+ Developers。管理门户的 Developer，以及 Developer 关联的 Consumer。
+ Settings。
    - 配置门户的基本信息。
    - 控制门户中 Developer 的注册审批是否自动通过、API Product 订阅是否自动通过。
    - 门户支持的三方登录。支持标准的 OIDC 配置，如 Aliyun、Google、Github 等。

### 创建 API Product
选择【API Products】-【创建 API Product】，创建一个 demo-api 的 API Product。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756120817284-7d572a58-15b2-41ca-8798-f8514dd48fec.png)

API Product 的初始状态为“待配置”，需要进行 Link API、发布到门户等操作。

### 关联 API
![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756135284300-92fbfc3d-4927-42d2-91eb-ef586a2ae083.png)

关联一个网关的 MCP 服务，数据源来自于 AI 网关 MCP 服务管理。API Config 也会自动同步 Higress 中的配置。

### Usage Guide
![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756135522434-60d162f9-3007-487d-a19b-36fc323e9bd3.png)

可以在使用指南中编辑自定义的文档信息。

### 发布到门户
在 API Product 准备就绪后，可以选择发布到指定的门户。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756135616759-69d1a672-cadc-40af-b329-0e1c712569bc.png)

至此，一个 Higress 的 MCP Server 成功发布到了门户。

## 三、HiMarket 门户
门户会有一个默认分配的域名，但域名解析需要用户自己完成，例如自动分配了 portal-68ac4564bdb292ee9261ff4a.api.portal.local 域名，需要将其解析到 api-portal-frontend 对应的 IP 上。

由于刚刚已经额外配置了 localhost 域名给测试门户，所以也可以直接通过 localhost:5173 访问前台。

### 注册 Developer 开发者
![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756136341614-78e70a99-6165-4ef0-9c7d-839538f32651.png)

由于门户之前在设置中未打开自动审批，注册账号后需要等待管理员后台审批开发者通过，审批通过后，方可使用注册的账号在前台登录。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756136953577-e7259989-49b7-4fb3-9ac3-023453fcb303.png)

访问 MCP 门户可以看到刚刚发布的 MCP Server

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756137076017-354e06c7-62e2-458b-bc08-c958f3b0d00d.png) ![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756137117244-4ffdaabe-3459-43df-a79f-a3a74fcb8641.png)

### 创建 Consumer 消费者
在 AI 开放平台的设计中，消费者 Developer 代表一般的用户身份，而用户需要持有对应的凭证才可以申请订阅 API Product，而凭证这一概念，在 AI 开放平台中称之为 Consumer 消费者，Developer 与 Consumer 是一对多的关联。

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756137408652-ececcf32-15a6-4b8d-a76a-6a8c6795e49a.png)

创建消费者之后，即可申请 API Product 的订阅

![](https://intranetproxy.alipay.com/skylark/lark/0/2025/png/156306/1756137466611-a90abc4b-7f39-4427-9443-671ddd24b5de.png)

门户的默认配置中，订阅的审批是默认关闭的，即开发者申请后会自动审批通过。

### 发起调用
携带消费者的凭证，配置门户中 MCP Server 的连接地址，即可发起对 MCP Server 的调用。 

