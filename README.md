# usermes
PC微信机器人之实战分析通过wxid获取用户信息
QQ：2645542961
本节主要分析通过wxid获取微信用户信息call，实现的思路是，首先附加微信到CE，在微信文件助手窗口，用CE搜索filehepler ，然后在PC微信切换到另外一个人的聊天窗口，看CE上面微信id变化，获得到当前人的微信id，在CE再次过滤，最后找到切换聊天窗口时，CE上面的微信id也一直变化，把这些移到下面，
![image](https://user-images.githubusercontent.com/73727649/119217004-d8b87880-bb09-11eb-9aac-e8ca74a4d200.png)

然后把微信id和昵称记录下来，再找个用户的微信id和昵称记录下来，
最后筛选出4个地址，然后比较笨的办法是一个一个在OD里面试，也有一个技巧，一般连着的地址不是的，所以选择第一个，跟其他三个不连续的，去OD里面下一个内存访问断点。
![image](https://user-images.githubusercontent.com/73727649/119217008-e3730d80-bb09-11eb-889a-3e4f4adbe05f.png)
![image](https://user-images.githubusercontent.com/73727649/119217011-e66dfe00-bb09-11eb-9918-605ab6528714.png)

然后在微信随便点击某个人的微信，看到OD断下来了，之后删除内存断点，然后在堆栈里面找，
然后找到一个从函数的开头，设置一个断点，然后在微信点击某个人的信息，看到是可以断下来了，然后跟踪断点，调试一下，
![image](https://user-images.githubusercontent.com/73727649/119217035-1ddcaa80-bb0a-11eb-91f5-07ca860032a3.png)
跟着断点走，找到疑似获取个人信息的函数，备注下来，同时下个断点，然后再点击某个人一个微信，看到这个断点断下来了，然后看下edi里面装的什么东西，发送是个缓冲区，看下这个缓冲区的大小是3DC，
![image](https://user-images.githubusercontent.com/73727649/119217136-af4c1c80-bb0a-11eb-833d-e7bcd03de61e.png)
找到缓冲区和wxid，一个是可以接收他数据的，另外一个是你获取那一个微信的信息。然后把没用的信息nop掉从新运行一次，如果不崩溃，就不需要传这些参数，
![image](https://user-images.githubusercontent.com/73727649/119217143-b4a96700-bb0a-11eb-8be8-d052bfb2ce31.png)
通过调试得到三个call，然后把偏移计算一下，复制call的地址，减去模块的地址，得到偏移地址。
![image](https://user-images.githubusercontent.com/73727649/119217147-ba06b180-bb0a-11eb-9cd2-87d9b8ce557d.png)
QQ：2645542961
目前已经实现了大部分功能，运行稳定，比如：发各种消息，接收各种消息，群管，下载文件，加好友，检测僵尸粉等等功能，可提供接口，方便二次开发，欢迎技术交流。
![image](https://user-images.githubusercontent.com/73727649/119217162-d4408f80-bb0a-11eb-9833-10edfae22073.png)
