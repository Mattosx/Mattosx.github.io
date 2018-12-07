---
title: quantumult
date: 2018/12/07 20:46:25
---

## IOS

在IOS上有三个比较主流的梯子，[surge](https://itunes.apple.com/us/app/surge-3/id1442620678?mt=8) [quantumult](https://itunes.apple.com/us/app/quantumult/id1252015438?mt=8) [shadowrocket](https://itunes.apple.com/us/app/shadowrocket/id932747118?mt=8) ，其中surge价格高昂，后面两个价格还是比较公道。quantumult 支持的协议也比较多，55、55R、VMess ( websocket + TLS) 等。下面教程主要摘自 [telegram channel](https://telegram.me/lhie1x) ，因为没有梯子访问不了的缘故，就搬到这个博客上了。

### 添加服务器

进入Settings（设置）-> 服务器, 可以使用 + 号或二维码扫描的方式添加，一般推荐自己搭建服务器。

More info: [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) [V2ray](https://www.v2ray.com/)


### 开启HTTPS 解密（可选）

大致过程就是生成自己的证书，利用中间人解密HTTPS，在拦截广告的时候特别有用。

1. 开启解密，生成证书
Quantumult：Settings - HTTPS - HTTPS Decryption

2. 信任证书：
设置 - 通用 - 关于本机 - 证书信任设置 - 信任


### 添加规则

quantumult 提供的功能可以制定非常完善的规则，本着不重复造轮子的心态，这里直接用过了[github](https://github.com/lhie1/Rules/blob/master/README.md) 上维护得比较好的规则设置。

1. 进入Settings（设置） — Favorites（订阅）

2. 选择右上角添加，Server（服务器订阅），Filter（分流），Rejection（链接阻止）

3. 选择 Filter （分流），Name（名称）随意，URL（链接）填入 https://raw.githubusercontent.com/lhie1/Rules/master/Quantumult/Quantumult.conf  ，看需求勾选个性化

4. 如果开启了HTTPS解密进入第5步，HTTPS解密【非必要】

5. 选择 Rejection（链接阻止） ，Name（名称）随意 ，URL（链接）填入 https://raw.githubusercontent.com/lhie1/Rules/master/Quantumult/Quantumult_URL.conf , 勾选Including Host Names（包含主机名）

6. 【规则生效或者更新规则】：在Favorites（订阅）的Filter（分流）订阅和Rejection（链接阻止）订阅多了订阅的选项，左滑选项，选择Replace（替换）替换，选择相应的服务器或者Policy（策略）

7. 在Filter（分流）替换过程中，注意选择【Apple、PayPal服务】是选择直连或者代理，包含iCloud服务和App Store下载等。其他如果不需要默认选项即可

8. 需要指定更多服务的Policy（策略）？添加Filter（分流），必须勾选个性化，操作不是Replace（替换），而是【Append】（增加）。地址：https://raw.githubusercontent.com/lhie1/Rules/master/Quantumult/Quantumult_Extra.conf ，小众的Policy包括：TVB, Viu, Spotify, BBC, Hulu

9. 服务器订阅即选择Server（服务器），输入名字（随意），填入地址，点击右上角Save（保存）。同规则一样，左滑选项，选择Update（更新）即可。

说明：Filter（分流）是为了分流或指定代理或直连，也能屏蔽广告。Rejection（链接阻止）是补充屏蔽广告用的。


### 策略生效问题

最热门的问题之一，我新建的策略为什么不生效？
首先新建策略是一个步骤，使其生效是另一个步骤。

如何使其生效？

在说方法之前先来说清楚APP是如何识别规则和策略，先举个简单的例子HOST-SUFFIX,wikipedia.org,PROXY。
先来分解一下这条规则的意思吧，HOST-SUFFIX这是规则的一种匹配方法，wikipedia.org是参数，PROXY是策略。

其中策略可以是自带策略中的PROXY、DIRECT、REJECT，可以是单个服务器节点，可以是自己新建的策略（静态策略、延迟策略、环境策略）。
那当你新建策略时，并没有在规则的第三个位置指定策略，APP如何识别生效呢？

策略生效是批量替换的一个过程，先来说订阅规则的时候指定策略
当在设置—订阅的分流中左滑一个订阅，选择替换时会有一些选择，这些选择是什么意思呢？

DIRECT（直连）是订阅规则中含有DIRECT，因为DIRECT是APP预设策略，所以DIRECT下面已经帮你选择DIRECT，哪些规则是DIRECT呢？比如qq.com
REJECT（拒绝）是订阅规则中含有REJECT，因为REJECT是APP预设策略，所以REJECT下面已经帮你选择REJECT，哪些规则是REJECT呢？比如Google广告

在绝大多数情况下，我们新建设策略是针对代理部分。当然你可以针对REJECT部分去选择一个含有REJECT/DIRECT的静态策略，之后可以排除故障，这根据需求来设置。
比如你不需要新建策略，那就按照APP的设置来，在替换时PROXY/未知策略（比如选择Netflix，不懂的就不选）会默认选择PROXY，如果替换时提示选择DIRECT的就选择DIRECT。因为还有部分人是需要选择PROXY的。
举例，我新建了一个延迟策略（就是每10分钟选择最低延迟的策略组）需要生效的话就是替换时将本来默认选择PROXY的部分点击选择到新建的策略，不懂的话建议选择DIRECT的就选择DIRECT。
Quantumult会记住你的选择，如果下次更新规则（对，替换步骤也是更新规则）会帮你勾选上，提高效率。

PROXY在默认情况算是一个静态策略，可以在Switch面板中在所有节点中选择，当在设置—策略—PROXY打开开关选择节点保存后，PROXY就变身延迟策略了。

当你不能上网的时候，或者一个网站能用，另外一个网站不行的时候请检查各个策略中节点的可用性。

### 服务器切换问题

在添加了多个服务器以后，点击（设置）右边的图标即可切换服务器。

--------------------


## Android

[shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android/releases) 官方的版本做得比较好，就直接用了，需要选择 Latest release 版本比较稳定。设置过程也比较简单，这里就不阐述了。