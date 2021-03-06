---
title: OhMyStar2 产品方面的自问自答
date: 2017-11-08 01:41:16
tags: [随便瞎扯,产品思考,产品汪,胡思乱想]
---


本文属于自问自答，解释一些 OhMyStar2 的产品思路。

<!--more-->

**注意**

本文具有强烈的个人感情色彩,如有观看不适,请尽快关闭. 本文仅作为个人学习记录使用,也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/4.0/deed.zh_TW)范围内转载或使用,请尊重版权并且保留原文链接,谢谢您的理解合作. 如果您觉得本站对您能有帮助,您可以使用[RSS](https://iiiyu.com/atom.xml)方式订阅本站,这样您将能在第一时间获取本站信息.


## Q & A

### 问题1: OhMyStar2 的愿景是什么？
OhMyStar 的愿景是做最好的 GitHub Star 管理工具。说句不要脸的话：从 OhMyStar1 开始，其实 OhMyStar 系列一直都是市面上能见到最好用的 Github Star 管理工具。

当然形成这个局面的并不是 OhMyStar 多么优秀。而是 OhMyStar 出现的比较早，让我有足够的时间去思考和设计整个产品。并且这是一个非常非常小众的需求，导致市面上没有太多类似的产品。以至于一些竞品的出现，都带有浓重“致敬” OhMyStar 的感觉。我并没有看见有更加优秀的方案来解决 Github Star 的管理问题。

### 问题2：OhMyStar1 存在哪些问题？

#### 1. 分组和 Tag 功能重叠。不够简单直观。
分组和 Tag 功能合并为一个。经过思考以后认为 Tag 比分组更好。因为单同一个项目需要给多个分类定义的时候。多 Tag 给用户的感觉比多个分组要准确的多。

#### 2. 软件功能较少，用户粘性不够好。
软件功能不够，就增加一些使用频率高的功能。Trending、搜索和 Readme 阅读模式。

Trending —— 每天、每周、每月各个语言多项目热度是一个很高频的使用场景。而 GitHub 官方几次改版都越来把它隐藏的更加深。我能理解官方的意图是让你自己多写代码，少去无意义的参合热点。但是浏览 Trending 是很多人每天的习惯，所以一定加上。

搜索 —— 是我在 OhMyStar 1 上使用最频繁的功能。能让我坚持写完 OhMyStar1 的一个重要动力就是当时 GitHub 官方对自己 Star 过的项目搜索支持相当糟糕。同时 OhMyStar1 的搜索其实很弱鸡。所以这么重要的一个功能必须进化的更加强大。

Readme 阅读模式 —— 当在使用某个库需要参考 Readme 的时候。可能就会在浏览器和编辑器 or Shell 之间来回切换。Readme 阅读模式的设计是可以钉到所有窗口的最前面。让用户更加方便的去一边操作一边看 Readem。

#### 3. 一些 GitHub 轻度用户 Star 项目较少甚至连一个项目没有。完全没有使用 OhMyStar 的必要性。

解决这个问题这是在设计 OhMyStar2 中最重要的一点。为什么呢？如果用户 Star 越多的代码项目，他管理代码项目的需求就会越高。如果能解决这个问题，就可以源源不断的创造管理 Github Star 的需求。那 OhMyStar 就能源源不断的得到新用户。在意淫完 OhMyStar 要有无数用户的以后。最后的问题就呼之欲出——如何让用户不停的 Star 项目？

这是我认为 OhMyStar2 中，我最骄傲的产品设计部分。那么 OhMyStar2 是如何解决这个问题的？

1. 使用 OAuth 认证，获得 Star 和 Unstar 的能力。（PS：OhMyStar1 是不用登录的，输入一个用户名就行）
2. Trending 入口在第一个层级。只要打开了 OhMyStar 就能让你 Star 。
3. 增加分享&导入功能。这是我认为 OhMyStar 2 最厉害的设计。不但解决了增加 Star 项目的难题。还获得了互联网变现的入场券。扩展了 OhMyStar 的商业模式。具体功能就是，我在 Tag 上右键的菜单里面有一个分享选项。点击以后，会生成一个包括此 Tag 下的所有项目的网页。你可以把这个网页URL分享给任何一个人。然后，其他用户打开这个网页后，只要安装了 OhMyStar2，点击网页上的导入按钮。就一键导入了这个网页上的所有项目。从此以后，OhMyStar2 就不再是一个单机 App。 它获得了用户之间互动的功能。（PS：可惜我的运营不利造成了目前为止几乎没有人发现使用）

#### 4. UI 丑陋、老套、过时

1. OhMyStar2 的 Logo 著名 UI 设计师藻堂大大友情赞助的作品。2015 年就设计完成。
2. OhMyStar2 的大部分 UI 是花费了大价钱请高级设计师 YuXiao Chen 的作品。2016 年中就设计完成。
3. OhMyStar2 里面的编程语言图标，是我花了好几天整理出来的。大部分找不到 svg 版本的 icon 都是我一笔一笔画的。

因为设计稿的完成时间和最终 OhMyStar2 上架有很长的时间间隔。所以仔细看 OhMyStar 2 的话会有一点点设计貌似不是最新的 macOS 规范的感觉。不过相比较 OhMyStar1 来说。这简直是翻天覆地的变化了。（PS：我真的尽力了）

### 问题3：OhMyStar2 会不会有 iOS 版本或者其他平台的版本？

作为一个产品经理的时候，我当然希望 OhMyStar 有 iOS 版本，甚至全平台版本覆盖。不过鉴于真实资源的情况我只能说 iOS 是在计划内的。但是要同时保证最佳体验和数据同步的技术问题，OhMyStar 的 iOS 版本其实难度比想象中的大。不过可以保证的是，OhMyStar iOS 版本的整体产品设计比目前可以看到竞品的产品设计要好上不少。

### 问题4：OhMyStar2 是一个什么商业模式？

1. 采用免费基本功能+订阅解锁高级功能模式。这个貌似不用怎么分析了，已经是主旋律了。
2. 还记得我设计的分享网页导入导出功能么？曾几何时，我期望所有用户都积极分享自己觉得有价值的项目。然后造成巨大流量。大家争先恐后的分享和传播。这样 OhMyStar 就真的是一个互联网产品了。
3. 本来我还一直规划一个联盟：去他妈的老子就是永远不降价联盟。因为我认为现在 app 动不动就打折和促销其实是损害了开发商和用户双方的利益。特别是在打折之前购买的用户，心里面可憋屈了。所以具体操作方法大概就是，起草一份 LICENSE 。里面规定了只要使用这个 LICENSE 那这个软件就不能降价了。如果被发现违法协议降价了。那这个开发商就会进入“去他妈的老子就是永远不降价联盟”的黑名单里面，信用度全部丢失。然后这样一份“去他妈的老子就是永远不降价联盟”肯定具备话题传播性。在联合几个独立软件开发商。进行第一次发布。 OhMyStar2 也会获得足够的曝光。一举多得，一箭n雕。

**OhMyStar2 永不降价**
**OhMyStar2 永不降价**
**OhMyStar2 永不降价**

### 问题5：OhMyStar2 现在用户量多少，收入如何？

曾几何时，我以为 Free 以后会有大量的用户增长，其实并没有。
曾几何时，我以为 OhMyStar2 能帮我养家糊口，然而更加没有。

OhMyStar2 的日活和月活数量跟 OhMyStar1 几乎一样。

我觉得我还是算作一个比较良心的开发商。OhMyStar1 的用户都可以免费获得一年的 Pro。导致最核心的目标用户群体没有付费的理由。而且 OhMyStar2 由于我的能力问题，在运营和技术上都没有做的很好。所以尽管有不少好朋友义无反顾的订阅年费。

但现在的全部收入依然不够支付一年的服务器费用，少到我都不好意思说。所以从经济效益上来看，OhMyStar 亏损相当严重。

### 问题6: OhMyStar2 未来会怎么样？

首先，OhMyStar2 的产品规划进度，远远超出我拥有的开发资源。而我又是一个拖延症晚期，所以迭代速度可能很慢。但是不会立马死掉。

其次，如果当我决定不在维护 OhMyStar2 的话，我可能会选择开源整个项目。这样至少能留下点什么。

最后，其实上面也能看出来，OhMyStar2 其实没那么多人用。让我觉得没有特别的成就感。所以如果您恰好看到了这篇文章，恰好有使用了 OhMyStar2，恰好又觉得 OhMyStar2 还有那么一点用。那希望您能多多推荐给周围的朋友。我在此谢谢各位客官老爷了。

[OhMyStar2 下载链接](https://itunes.apple.com/us/app/ohmystar2/id1218642292?ls=1&mt=8)

## 后记

其实这篇问答在没有发布的时候，我就已经规划在发布应用以后立马写出来进行推广传播。可惜懒癌晚期啊。拖拖拖，拖到了大半年以后。所以很多细节想法和具体取舍决策有模糊不清的感觉。想好好说说，却又杂乱无章。

在原有的推广规划里面，接下来我应该还会再写一篇 OhMyStar 的技术问答，说一说哪些躺过的坑。希望自己能尽快写出来吧。
