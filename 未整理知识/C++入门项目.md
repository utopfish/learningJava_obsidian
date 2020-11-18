## [FreeNOS](https://github.com/Neirth/FreeNOS)
一个用C++实现的微内核的操作系统，各种宏内核中的服务作为一个独立的services在微内核中，基于消息的通信方式，这点其实跟mac内核中的mach那部分机制相似。
代码风格，注释，doxygen，scons，设计模式。
直接入手就啃Linux kernel的同学，个人觉得会门槛有点高，身体还不是特别棒的时候，直接攀登珠穆拉玛还是有点吃力的，可以先找个黄山爬爬。

## Chromium

向左向右，怎么看现在只有Chromium是c++中最庞大的一个，自己在造轮子的时候，可以搜索下这个宝库，保证有各种型号的轮子。Chromium涉及了几乎所有的平台，所以不仅能学到平台之上API的封装，还有包括Network的各种封装和借口，协议的C++的实现。更甚至，webrtc，native client，sandbox，GPU，并行加速，debug，各种神奇的第三方的库，各种各样的技术，能潜心学深一个，就可以功力大增。

## [Sandstorm](https://github.com/sandstorm-io/sandstorm)

Github上面最近比较火的一个项目，主页上面一句话的介绍“Personal Cloud Sandbox”。主要解决的问题是，在自己的私有人服务器上面，安装各种开源的云服务(GitLab, WordPress......),就像mobile上面的通过应用商店install app那样快速方便，实现这个目标，要在Linux之上，构建一个基于sandbox的平台，并且留给开发者porting这些web 服务 (.spk file)到应用商店。

## [BitCoin](https://github.com/bitcoin/bitcoin)
我们可以利用的是背后的P2P Protocol，密码学，支付，去中心化这些。
现在有很多衍生的开源项目，就是利用BitCoin 的这套P2P的框架在做，bitmessage，
[bitmessage](https://wiki.bitmessage.org/Main_Page)