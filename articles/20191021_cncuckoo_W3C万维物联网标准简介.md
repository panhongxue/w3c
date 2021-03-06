## W3C万维物联网标准简介

10月20日，第六届世界互联网大会在浙江乌镇开幕。国家主席习近平致贺信。

> 习近平指出，今年是互联网诞生50周年。当前，新一轮科技革命和产业变革加速演进，**人工智能、大数据、物联网等新技术新应用新业态方兴未艾**，互联网迎来了更加强劲的发展动能和更加广阔的发展空间。发展好、运用好、治理好互联网，让互联网更好造福人类，是国际社会的共同责任。各国应顺应时代潮流，勇担发展责任，共迎风险挑战，共同推进网络空间全球治理，努力推动构建网络空间命运共同体。
>
> （新华社北京10月20日电：http://www.xinhuanet.com/politics/2019-10/20/c_1125127764.htm）

社交媒体实现了“人联网”，在线支付实现了“财联网”。如今，人工智能、大数据技术的发展正有力地推动“物联网”的兴起。不难想象，物联网将在智能家庭、产业、城市，乃至智能国家、智能地球的建设中发挥基础性作用，为未来的人类和社会造福。

然而，由于联网设备的制造厂商不同、接入互联网使用的协议不同，如今的物联网（IoT，Internet of Things）依旧呈现“一盘散沙”、“各自为政”的状态。IoT平台和生态虽然层出不穷，快速演进，但各家协议栈和软件应用之间缺乏互通，形成了千千万万个专有IoT“孤岛”。W3C称IoT的这种状态为“碎片化”（fragmentation）。碎片化导致用户与供应商绑定、开发交付成本高昂、软件组件难以复用，这些问题成为阻碍IoT向更深、更广领域应用迈进的巨大障碍。

为实现IoT基于Web技术的互联互通，W3C决定在已有IoT平台/生态基础上“做一些填补空隙”的工作，并于2015年1月成立WoT兴趣组（https://www.w3.org/2019/07/wot-ig-2019.html），又于2016年12月成立WoT工作组（https://www.w3.org/2016/12/wot-wg-2016.html），正式开始WoT（Web of Things，万维物联网）标准的制定。

截止到2019年5月，WoT工作组已经发布5份文档，其中2份已经进入“CR”（Candidate Recommendation，候选推荐）状态：

> **注意**
>
> 上述这些规范文档并不是最终标准。W3C标准的最终状态是“REC”，即Recommendation（推荐标准）。在到达“REC”阶段之前，以上所有规范内容都可能被修改，并且也不排除某份文档被废弃的可能。换句话说，以上规范文档均不能作为正式实现文档在产品中使用。

| 规范                                    | 当前状态           |
| --------------------------------------- | ------------------ |
| WoT Architecture                        | CR                 |
| WoT Thing Description                   | CR                 |
| WoT Scripting API                       | WD，Working Draft  |
| WoT Binding Templates                   | Working Group Note |
| WoT Security and Privacy Considerations | Working Group Note |

本文将对上述W3C公开发布的规范进行概要介绍，仅供参考。

## 1. WoT架构

- WoT Architecture（候选推荐）：https://www.w3.org/TR/wot-architecture/
- WoT Architecture（编辑草案）：https://w3c.github.io/wot-architecture/

物体，指的是某个物理或虚拟实体（如设备或房间）的抽象表示，由标准化的元数据描述。在W3C WoT中，描述元数据**必须**是WoT Thing Description（TD）。消费者**必须**能够解析和处理基于JSON的TD。

![](https://p5.ssl.qhimg.com/t0128d98ca952c840a4.jpg)

**WoT物体**

WoT物体描述是标准化的、机器友好的表示格式，消费者可以通过这种格式发现和解释物体的能力（通过语义注解），在与物体交互时适应不同实现（如不同的协议和数据结构）， 而达到跨IoT平台（如不同的生态或标准）互操作的目的。

![](https://p4.ssl.qhimg.com/t014de300e175cd67e3.jpg)

**消费者与物体交互**

物体必须托管在联网的系统组件上，基于一个软件栈通过（面向网络的）WoT接口实现交互。

互联网无法访问本地网络是实际部署中面临的一个典型问题，通常是因为存在IPv4 NAT（Network Address Translation）或防火墙。为解决这个问题，W3C WoT允许物体与消费者之间存在中介层（intermediary）。

![](https://p4.ssl.qhimg.com/t01bd060bc84401863b.jpg)

**中介层**

中介层（intermediary）可以充当物体的代理，此时中介层拥有与背后的物体类似的WoT物体描述，但指向的是由这个中介层提供的WoT接口。中介层也可以给背后的物体添加额外的能力或者将多个可用的物体组合起来，从而构成一个虚拟实体。对消费者而言，中介层看起来就像一个物体，因为它们拥有WoT物体描述，而且提供WoT接口。

W3C WoT的概念适用于IoT应用的各个层面：设备层、边缘层和云计算层。这样可以促进跨层级通用接口和API的产生，进而促成IoT应用间不同的交互模式如物与物、物与网关、物与云、网关与云，甚至云联盟（如两个或多个服务提供商的云计算环境互联）。

![](https://p3.ssl.qhimg.com/t01a3a7d1d3aaa3eb82.jpg)

**WoT的抽象架构**

## 2. WoT物体描述

- WoT Thing Description（候选推荐）：https://www.w3.org/TR/wot-thing-description/
- WoT Thing Description（编辑草案）：https://w3c.github.io/wot-thing-description/

WoT物体描述（TD）基于一个语义词汇表定义了一个信息模型，同时基于JSON定义了一种序列化表示。TD提供有关物体的丰富的元数据，这些元数据既对人类友好，也对机器友好。

物体描述（TD）用元数据来描述物体的实例，包括名称、ID、说明等，还可以链接到相关物体和其他文档以提供相关元数据。TD中也包含（基于WoT架构定义的交互模型的）交互可识别功能的元数据、公共安全配置元数据和定义协议绑定的通信元数据。可以把TD看成物体的index.html，因为它为了解物体提供的服务及其相关资源提供了一个入口，而物体提供的服务及其相关资源都使用超媒体控件来描述。

理想情况下，TD由物体自己创建并/或托管，可以从物体目录检索到。不过，在物体资源有限（如内存有限、电量有限）或将既有设备改造为WoT时，TD也可以托管到外部。将TD注册到一个目录是改进物体发现（如对于受限设备）和方便设备管理的一个常见做法。消费者同时采用一种TD缓存机制和一种通知机制（在物体更新时，可以通知消费者获取新版本的TD）是推荐的做法。

为实现语义上的互操作，TD可以利用领域特定的词汇表，并明确提供扩展入口。不过，制定领域特定的词汇表已经超出当前W3C WoT标准化任务的范围。

总的来说，WoT物体描述组件从两方面增进互操作能力。首先，TD让WoT实现机器与机器通信；其次，TD本身可以作为一个通用、统一的格式，让开发者记录和获取所有必要细节以创建应用去访问IoT设备和利用其数据。

## 3. WoT绑定模板

- WoT Binding Templates（备忘录）：https://www.w3.org/TR/wot-binding-templates/
- WoT Binding Templates（编辑草案）：https://w3c.github.io/wot-binding-templates/

IoT使用很多不同协议访问设备，因为没有一种协议是通用的。为此，WoT最重要的挑战就是让各种不同的IoT平台（如OCF、oneM2M、OMA LWM2M、OPC UA）和不符合任何特定标准但可以基于合适的网络协议提供够用接口的设备之间能够对话。WoT应对这种多样挑战的方案就是协议绑定，但必须满足一些约束条件。

非标准性的WoT绑定模板规范提供了一套通信元数据蓝图，以指导如何与不同的IoT平台交互。在描述一个特定的IoT设备或服务时，可以查询与相应IoT平台对应的绑定模板，获得为支持该平台而必须在物体描述中提供的通信元数据。

![](https://p3.ssl.qhimg.com/t01283365dabd02a8c6.jpg)

**从绑定模板到协议绑定**

上图展示了如何使用绑定模板。WoT绑定模板对每个IoT平台只创建一次，然后就可以在该平台所有设备的TD中重用了。处理TD的消费者必须实现必要的协议绑定，通过包含一个对应的协议栈并根据TD中给出的信息配置该栈（或其消息）。

协议绑定的元数据分五个维度：

- IoT平台

  IoT平台经常会在应用层引入专有的修改，比如特定于平台的HTTP首部字段或CoAP选项。表单中除了为应用层协议使用而定义的字段，还可能包含应用这些修改的必要信息。

- 媒体类型

  IoT平台交换的数据经常出现不同格式（也就是序列化方式）。媒体类型用于标识这些格式，而参数可以进一步明确格式。表单中除了已知的如HTTP的内容类型（包含媒体类型和可选参数，如`text/plain; charset=utf-8`）字段外，还可以包含媒体类型和其他可选参数。

- 传输协议

  WoT使用传输协议来指代底层、标准化的应用层协议，不包含应用特定的选项或子协议机制。表单（提交）目标的URI模式包含确定传输协议的必要信息，如HTTP、CoAP或WebSocket。

- 子协议

  传输协议可以有扩展机制，但必须已知可以成功交互。这种子协议不能在URI模式中体现，必须显式声明。相关的例子有HTTP的推送通知补丁如长轮询或服务端推送事件Server-Sent Events 。表单可以包含必要的信息以标识额外表单字段中的子协议。

- 安全

  通信栈的不同层次可以有各自的安全机制，而多个安全机制也可以一块使用，以互为补充。例子包括(D)TLS、IPSec、OAuth和ACE。由于安全是贯穿所有规范的，应用正确安全机制的必要信息可以在物体的通用元数据中给出。

## 4. WoT编程API

- WoT Scripting API（编辑草案）：https://www.w3.org/TR/wot-scripting-api/
- WoT Scripting API（编辑草案：新）：https://w3c.github.io/wot-scripting-api/

WoT编程API是W3C WoT组件中可选的“便利”组件，可以通过提供类似Web浏览器API一样基于ECMAScript的API让开发IoT应用更容易。通过在WoT运行时内集成一个脚本运行时，WoT编程API可以让编写可移植应用脚本成为可能，这些脚本定义物体、消费者与中介层的行为。

过去，IoT设备逻辑都在固件中实现，这就导致了类似嵌入式开发的低效率，而且升级过程也相对复杂。现在，WoT编程API让一个IoT应用的设备逻辑可以通过在运行时中执行可重用脚本来实现，与在Web浏览器中运行脚本没什么不同，因此可以提升生产力，降低集成成本。不仅如此，标准化的API也让开发可以移植的应用模块成为可能。这样，把计算密集型逻辑从设备迁移到本地网关，或者把时效性要求高的逻辑从云端迁移到本地网关或其他边缘节点就都很容易了。

非标准性的WoT编程API定义了相应编程接口的结构和算法，让脚本可以发现、获取、消费、生成和暴露WoT物体描述。WoT编程API的运行时负责实例化本地对象，作为访问物体及其交互可识别功能（属性、动作和事件）的接口。同时脚本也可以通过运行时暴露物体，即定义和实现交互可识别功能并发布物体描述。

## 5. WoT安全与隐私指南

- WoT Security and Privacy Considerations（备忘录）：https://www.w3.org/TR/wot-security/
- WoT Security and Privacy Guidelines（编辑草案）：https://w3c.github.io/wot-security/

安全是贯穿所有规范的主题，系统设计的方方面面都要考虑。在WoT架构中，某些显而易见的特性都有安全保护措施，比如TD中支持公共安全元数据、设计WoT编程API时分离关注点。每个组件的标准也都包含各自安全与隐私的议题。

## 写在最后

W3C是开放的国际标准组织，所有工作组标准的制定都在Github上公开进行。如果你对正在制定中的WoT标准感兴趣，欢迎通过下面的链接找到并参与ISSUE讨论，帮助W3C制定出更好的Web标准。

## 相关链接

- WoT Architecture：https://www.w3.org/TR/wot-architecture/
- WoT Thing Description：https://www.w3.org/TR/wot-thing-description/
- WoT Scripting API：https://www.w3.org/TR/wot-scripting-api/
- WoT Binding Templates:https://www.w3.org/TR/wot-binding-templates/
- WoT Security and Privacy Considerations：https://www.w3.org/TR/wot-security/
- WoT兴趣组：https://www.w3.org/2019/07/wot-ig-2019.html
- WoT工作组：https://www.w3.org/2016/12/wot-wg-2016.html

