# hookdll
这是windows下指定版本微X的hook工具用的dll文件。专为3.2.1和3.3.0版的微X用的。\
工具（注入器）的就从 https://github.com/cixingguangming55555/wechat-bot/tree/release/server 下载。\
上述两个版本的是经过本人确认可以用的。另外，3.6版的我试过一次，但不行。其他版本未知。\
具体使用方法很简单，我用的是Python语言，下面就python的实现来简要说一下。

在windows下安装了上述2个版本微X的任意一个后，登录微X，然后运行hook工具，这个时候它会显示当前的微X版本，在DLL文件下拉框中选择对应版本的dll，点注入DLL就完成了。接下来通过websocket-client就可以收发消息了。

这样做相当于你有了一个跟微X通信的websocket 服务器端。然后再搞一个websocket client端，通过client与websocket server通信，它帮你转发给微X，就是这个意思。client端这个稍微要写一些代码，大的框架可以参看上面这个github的client目录下的各种语言的sample代码。我自己的就是参照python目录里的代码，接收消息和发送消息人家都给写好了，我们自己要做的就是在收消息后和发消息前再加上一些自己的处理所必须的代码就OK了。
记得在安装完微X后在设置里把自动更新的勾给去掉。另外，在windows server上安装后可能跑不起来。正常的windows 10或者windows 11都没有问题的。

另外，我自己遇到的一个比较麻烦的问题。就是上述环境全部搞好，运行起来后一直开着，我把websocket收到的每条消息在运行python的那个命令行窗口中打印出来（就管这个叫打log吧）。\
但有的时候似乎发生了堵塞，就是微X上收到的消息，在Log里没被打印出来，按理说log和微X窗口应该是同步显示才对。但是当我在那个命令行窗口敲一下回车，log就哗哗哗的又显示出来了。\
这个现象是随机发生的，没有任何规律可言。可能是运行python程序的几十分钟后，几个小时后，或者几天后。而且什么时候开始发生堵塞也不知道，当发现的时候可能已经堵塞好几个小时了。\
试着在run_forever函数里加里心跳和timeout设置（如下）后也还是会出现，只不过似乎频率变低了。但一直没有完全解决。如果有哪位解决了这个问题，请一定告知，多谢！\
run_forever(ping_interval = 180, ping_timeout = 179, reconnect = 2 ) 

追记：终于找到上述问题的类似案例， https://github.com/sammchardy/python-binance/issues/268 人家说是命令行窗口自己的问题。说只要把 快熟编辑模式 去掉就OK了。试了一下，这个选项是要在每次启动命令后都要改，并不是同一个窗口就只改一次就OK的。具体地说，同一个程序跑起来以后设置了。等它结束后，就是是在同一个命令行窗口再执行，也还得再改一次设定。

有其他问题可一起讨论。加WX: Roby_Shi 由于有些实际的应用，想要代码的就不要来联系了。
如果您觉得本文对您有用，可以请作者喝杯咖啡。谢谢

![wechaPay_S2](https://user-images.githubusercontent.com/37259748/219905856-1ef59988-fb55-486d-b6d0-bbcbec3d881f.jpg)
