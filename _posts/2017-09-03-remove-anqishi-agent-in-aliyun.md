---
layout: post
title: 卸载阿里云的安骑士 Agent
category: tech
tags: linux aliyun
---
![](https://cdn.kelu.org/blog/tags/aliyun.jpg)

对于黑盒子的商业性的软件，如果不是非常信任的厂家，最好还是不要随便让别人装在自己的服务器上。

所以像阿里云这种安骑士 Agent，是一定要卸载的。

好在官方也提供了卸载方法，如下：

1.  执行以下命令下载安骑士 Agent 卸载脚本。

    `wget http://update.aegis.aliyun.com/download/uninstall.sh`

2.  依次执行以下命令卸载安骑士 Agent。

    `chmod +x uninstall.sh`
    `./uninstall.sh`


# 参考资料

*   [安骑士](https://www.alibabacloud.com/help/zh/product/28449.htm "安骑士")
*  [卸载Agent](https://www.alibabacloud.com/help/zh/doc-detail/31777.htm)