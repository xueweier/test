---
layout: post
title: 玩心马宗骥：游戏持续“霸榜”幕后的数据真相 | 转自神策数据
category: product
tags: operations
---
![](https://cdn.kelu.org/blog/2017/09/data1.jpg)

精细化运营的文章。记录在小站时常学习。以下是原文：

精细化运营对游戏玩家新增、留存、转化、收入等各环节都起到极大促进作用。从游戏复杂、琐碎数据中挖掘数据价值，不断优化游戏机制设计、场景设计、故事情节设计、位置和物品设计、界面设计等，是游戏产品持续优化与改进、平台精细化运营的加持。

玩心（上海）网络科技有限公司（下称“玩心”）是一家专注于网络游戏研发的互动娱乐企业。玩心一直坚持创造性的产品设计理念、拥有强大的产品研发能力。首款作品《迷城物语》于2016年8月24日AppStore首发，在10月初攀升到AppStore畅销榜前列。2016年11月末《迷城物语》海外版本陆续在香港、澳门、台湾、新加坡、马来西亚等地上线，首日即在各地区AppStore和GooglePlay商店登顶并持续霸榜。

![](https://cdn.kelu.org/blog/2017/09/data2.png)注：《仙境迷城》、《沉睡森林》为《迷城物语》不同地域名称

将数据驱动理念贯穿游戏设计与测试的全流程中，保证顺畅、优质的玩家体验是《迷城物语》成功的关键因素。为保证将更多精力集中在游戏的研发中，玩心团队开始选型第三方数据分析平台。作为资深的游戏研发人员，《迷城物语》技术负责人马宗骥对此有着高标准的考量。近日，马宗骥为笔者介绍游戏行业的数据驱动价值，围绕数据采集准确性、数据分析与决策的科学性、数据资源安全性等方面分享选型经验，观点深入浅出，具有一定的行业参考价值，如下。

科学数据采集：后端埋点更全、更准、更灵活 数据采集是构建数据平台的核心要素，不仅可以监控游戏数据的整体健康状况，更是对玩家行为进行深度分析的有力保证，从而提高玩家体验。那么，依靠数据优化玩家体验，什么才是数据采集的最优方案？

众所周知，前端埋点已是各行业成熟且广泛采用的数据接入手段，对于分析前段页面是否合理，分析在后端没有交互的前端行为等，必须采用前端埋点。

然而，对游戏行业而言，单纯前端埋点存在一些致命弊端。结合游戏的实际应用场景，马宗骥介绍一二。

1、玩家行为数据前端采集不全面、不准确，错误的数据易导致错误的决策。

以 PCU（最高同时在线人数）统计为例。在网络游戏领域内，PCU 是游戏系统运维中衡量系统运行压力的重要指标，也是游戏受欢迎程度的考量指标。然而，有时玩家已经退出游戏，但是连接还在，此时前端采集是不准确的。这样的数字不能正确衡量服务器机器的负载情况、数据库的压力情况等。如此在后端（服务器）埋点最为精准，数据延迟和丢失的情况几乎不存在。

2、前端埋点无法应对灵活游戏场景的分析需求。

如果要调整玩家行为事件，如任务副本及其它们的属性采集方案，前端埋点需要修改客户端的代码，则需要一定的时间周期来发布新版本。马宗骥介绍：“NPC（非玩家控制角色）状态、副本状态、经济系统实时状态等统计类数据，这些数据使用前端埋点是无法统计到的，而在后端采集数据可根据实际情节灵活完成数据统计工作。” 

因此，游戏行业数据采集方案不能忽略“后端埋点”方式，而在考量市面主流基于用户行为的数据分析产品时，马宗骥发现，除神策分析外，绝大多数厂商的数据接入方案中均采用前端埋点方式。他认为，神策数据支持全端数据打通，数据采集精准、灵活，且符合游戏特定场景下的定制化需求，合理的数据埋点才能实现严密的科学分析与决策。

提升活跃、留存：如何精准找到玩家“流失点”？ 全面、精准、灵活的数据采集方式，为《迷城物语》后续实现科学分析玩家活跃度、留存率，以及优化付费体系等提供坚实的数据基础。

游戏的生命周期的时长差异、玩家的游戏粘度，直接体现了游戏的竞争能力和盈利能力。近日Adobe公司发布的一份行业报告显示，在所有APP类型中，游戏是用户抛弃率最高的类别。玩家对游戏的直观感受、游戏难度曲线、游戏节奏的松弛、游戏福利等游戏内涵都能够导致游戏玩家流失。正确找到玩家流失原因，是促进玩家活跃、挽留玩家的第一步。《迷城物语》是如何做的？下面为《迷城物语》在删档测试期间的相关应用情景。（注：以下配图所涉及的数据，均为模拟真实应用场景下的虚拟数据）

在神策分析的平台上，可统计流失玩家的等级分布，判断玩家流失与关卡设置的相关性。

![](https://cdn.kelu.org/blog/2017/09/data11.jpg)

图1 玩家在首次登陆游戏之后的8周流失情况分析

上图显示，100～110级、80～90级是玩家流失较多的关卡。为精准导致玩家流失的关键因素，需要每个环节、具体场景进行深入追踪与分析。

通过下图我们发现，在100～110级流失的玩家所操控的角色大多停留在“打怪”动作上，机械地打怪练级,玩家开始感觉枯燥甚至疲惫。找到这一“流失点”后，《迷城物语》运营人员可以适当调整该关卡的怪物数量，并增加新鲜因素，从而平衡游戏趣味性和玩家精力。

![](https://cdn.kelu.org/blog/2017/09/data12.jpg)

图2 100～110级玩家的完成任务平均次数、挑战副本的平均次数、打怪平均次数对比

![](https://cdn.kelu.org/blog/2017/09/data13.jpg)

图3 在80～90级流失的玩家，参加活动的平均次数、挑战副本的平均次数、完成任务的平均次数情况

下面重点分析80～90级流失的用户，通过处于该级别的玩家行为事件分析，我们发现，该阶段的玩家任务完成率很低。说明在这一阶段玩家没有任务，挑战性的缺失让玩家丧失目标感，降低玩家体验。因此在此阶段应优化游戏任务，并加强游戏任务引导工作。

提升收入：巧妙设置游戏付费墙 商业分析机构Venture Beat基于手游调查报告显示，在所有付费玩家中，49%的玩家在进入游戏第一个月内付费，53%的玩家在首次付费后14天内再次花钱。可见，玩家首冲后，二次充值的概率高于没有过充值记录的玩家。马宗骥也表示：让玩家第一次开始付费对于运营商来说至关重要。 ![](https://cdn.kelu.org/blog/2017/09/data14.jpg)

图4 游戏充值的触发用户数级别分布

![](https://cdn.kelu.org/blog/2017/09/data15.jpg)

图5 20～30级的玩家消费的触发用户数

通过上图，可清晰看出玩家首冲等级分布，主要集中在20～30级，结合游戏本身情节设置，在该阶段中不少玩家由于“十连抽”资源赠送行为的结束，会选择充值购买宠物。在这个期间，游戏设计人员可重点优化此阶段的付费体验：优化玩家购买的入口以保证付费方便快捷；优化购物商店的广度和深度，将重要商品显著摆出，并将付费引导完善，保证首冲过程流畅。 首冲环节优化只是《迷城物语》搭建精细化游戏付费体系一小步。除了以上图片所涉及的关键指标，玩家高、中、低额度占比及变化、用户充值频次情况、上周/月付费本月继续付费占比、付费率、付费引导使用率及效果、ARPU（每用户平均收入)）ARPPU（每付费用户平均收益）、最高付费情况等，都是衡量买家付费能力的重要指数，《迷城物语》通过神策分析，事先将这些关键指标埋点，追踪玩家行为并将玩家分群，从而优化不同玩家群体的游戏体验，进而做出针对性的付费功能调整和优化。

安全，还是安全 除了支持后端数据采集、丰富指标体系与多维分析等优势外，神策数据支持私有化部署也是玩心最终选择神策数据的关键因素。马宗骥对数据安全问题一直有很大顾虑，他强调：“保护玩家的隐私数据安全是作为游戏开发商的基本义务和责任。”神策数据为其搭建了私有的数据平台，保障了玩家的数据不被第三方获取。

《迷城物语》所践行的只是数据驱动的多元化应用的冰山一角。在搭建全面科学的数据基础后，实现玩家活跃度与留存率的提升，以及优化游戏的收费体系。签约至今，马宗骥如是总结：

**"从与神策数据的分析师与销售人员接触开始，无论是Demo展示、新手教程的文档，还是后续服务细节，很容易看出这是一家技术性导向型公司。神策分析为我们实现数据驱动提供了技术保障，节省了人力成本，让我们能够把精力集中在产品研发上，为玩家提供更优质、更好玩的游戏。公司不大，但真的很靠谱，期待下一款游戏更深入的合作。"**

# 参考资料

* [玩心马宗骥：游戏持续“霸榜”幕后的数据真相](https://sensorsdata.cn/blog/wan-xin-ma-zong-ji-you-xi-chi-xu-ba-bang-mu-hou-de-shu-ju-zhen-xiang-2)
