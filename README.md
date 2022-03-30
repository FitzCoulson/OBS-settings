# OBS-settings
It's a basic setting of OBS in order to improve the streaming experience.


# 认识OBS #
* OBS全称Open Broadcaster Software，它比各大平台自己的直播软件功能更强大
* 下载地址：https://obsproject.com/
* 下载后直接打开，根据步骤安装即可，在此不多赘述

* 安装完成后，关闭欢迎向导，就能看到OBS的主界面
![image](https://user-images.githubusercontent.com/42920883/160761271-1fe0e798-edb4-4686-b01f-a21c4de8d34a.png)


# 添加直播窗口 #
* 在“来源”窗口点击加号，即可添加直播来源
![image](https://user-images.githubusercontent.com/42920883/160761693-fba76479-99d3-4de8-a08a-42e783d4083a.png)

  * 图像：将一个静态图像添加到场景中
  * 图像幻灯片放映：将多张图片作为幻灯片进行放映
  * 场景：添加一个场景（可以在场景中嵌套场景）
  * 媒体源：将视频等媒体文件添加到场景中
  * 文本：将文本添加到场景中
  * 显示器采集：将显示器画面添加到场景中（你从电脑上看到什么，观众就能看到什么）
  * 浏览器：将浏览器窗口添加到场景（仅浏览器，后台也采集）
  * 游戏源：将游戏窗口添加到场景（仅游戏，后台也采集）
  * 窗口采集：将指定窗口添加到场景（仅窗口，后台也采集）
  * 色源：添加一个指定颜色的“窗口”
  * 视频采集设备：添加视频采集卡采集到的画面（使用外接摄像头或摄影机直播时）
  * 音频输入采集：添加音频输入（电脑外声音，如使用麦克风）
  * 音频输出采集：添加音频输出（电脑内声音，如使用电脑播放音乐）

* 以显示器采集为例，简单介绍一下使用方法：
  * 创建或选择源：
   * ![image](https://user-images.githubusercontent.com/42920883/160763359-3ab22e23-875d-438d-85df-b76cf2ba2910.png)
   * 选择新建（可以进行命名），点击确定
  * 配置属性：
   * ![image](https://user-images.githubusercontent.com/42920883/160763546-65b761c7-d979-4c30-86a6-94a47790b870.png)
   * 选择需要的捕获方式（默认自动即可）以及需要捕获的显示器（如有多个显示器，选择想要的即可），还可以选择是否显示鼠标等，点击确定
  
  * 此时红框内的画面即为直播场景画面，此时可以通过按住红点来缩放场景
  
  
# 笔记本黑屏解决方法（如果无此现象则跳过） #
* 如果笔记使用混合输出技术，可能会导致采集到黑屏画面
* Windows10 NVIDIA显卡：右键桌面，选择NVIDIA控制面板，在管理3D设置中找到程序设置，找到OBS，将图形处理器中改为集成图形，应用保存
* ![image](https://user-images.githubusercontent.com/42920883/160764958-58c27959-be2c-4b01-912e-9cf5b9492ef0.png)


# 将画面传输到直播间 #
* 在OBS的顶栏选择“文件”中的“设置”，选择“推流”选项，将服务设置为自定义。此时需要填写服务器和串流密码，此内容由直播平台提供。
* ![image](https://user-images.githubusercontent.com/42920883/160767955-c7e106b7-2e04-481e-a8ee-212dac894091.png)

  * B站直播：首先打开B站直播首页：https://live.bilibili.com/ ，选择右侧开播设置，完成封面和简介设置后，点击开始直播，此时便能看到你的服务器地址和串流密钥
  * 小红书、抖音、淘宝、快手、拼多多直播：这些平台不提供服务器地址和串流密钥，所以需要通过软件获取
   * 下载魔豆推流助手：https://www.modou.tv/?p=2134
   * 下载完成后安装打开（如果报毒则关闭杀毒软件）
   * 选择相应的直播平台后，输入账号密码登录，点击提取
   * ![image](https://user-images.githubusercontent.com/42920883/160767522-bd61367c-981d-4d3c-97f9-cdb982b61473.png)
   * 打开官方的直播助手后，开始直播
   * ![image](https://user-images.githubusercontent.com/42920883/160767729-3990ee40-e6e1-4e92-905a-e25501b6bdf9.png)
   * 此时官方直播助手会关闭，并获取到推流地址
   * ![image](https://user-images.githubusercontent.com/42920883/160767757-32ff64b0-309e-49cc-bcf8-3adfcac5a66a.png)

* 将获取到的串流地址和密钥复制到OBS的设置中，返回到主界面，在右下角选择开始推流，此时便可在直播间看到自己的画面
* 如果提示连接失败，则重新确认一下地址和密钥是否填错。亦或是长时间未操作导致直播平台将直播间设置为下播状态，则需要重新进行开播


# 优化直播画面 #
## 分辨率和帧率
* 打开OBS“设置”，选择“视频”，这里有两个分辨率选项，基础分辨率为OBS采集的画面范围，输出分辨率为观众看到的画面分辨率
* 一般来说，只需要调整输出分辨率。建议使用1920\*1080分辨率，如果为聊天、课程等直播内容可将帧率调为30，如果为户外、游戏等直播内容可将帧率调整为60

## 编码器和码率
* 在“设置”中的“输出”中，将“输出模式”改为高级。
* 编码器优先选择x264
* 在1920\*1080的分辨率下，30帧时应该将码率设置在4500kbps左右，60帧时将码率设置在6500kbps，如果为游戏直播，则将码率设置为10000kbps
* CPU使用预设，选项从低到高，越高画质越好，对CPU性能要求也会越高，默认为fast，是兼顾性能和画质的选项。

* 完成设置后返回主界面进行推流，如果右下角显示绿色，说明画面已经正常推送。如果显示黄色或红色，说明网速不够，需要降低码率
* 如果左下角显示编码过载，表示CPU性能不足，需要降低CPU使用预设，但不推荐低于very fast

* 如果CPU无法在very fast上正常推流，则考虑使用显卡推流
* N卡或A卡使用NVENC或AVC Encoder编码器，Mac用户不推荐使用VT编码器，兼容性不好
* 注：此为CPU性能不足时使用，优先仍选择x264编码器


# 优化直播声音 #
* 在混音器面板中，桌面音频是系统发出的声音（如正在播放的音乐），麦克风为收录的外界的声音
* ![image](https://user-images.githubusercontent.com/42920883/160772585-3118ba35-a9ab-4aff-948e-22f2cdbe5532.png)
* 右侧的齿轮按钮为设置，在两个音频中的设置里的属性，选择正在使用的设备

## 麦克风的调试：
* 简单的说几句话进行测试，如果指示条出现红色，则说明声音过爆，使用指示条下方的滑块将声音变小，或离麦克风稍远一些

## 解决MacOS无法捕捉桌面音频（如果无此现象则跳过） 
* 登录网站:https://existential.audio/blackhole/
* 填写信息后下载Blackhole
* 安装完成后进入“音频MIDI设置”应用程序
* ![image](https://user-images.githubusercontent.com/42920883/160774128-f1337877-d417-46e5-8519-c5789c5a576c.png)
* 点击左下角的“+”选择“创建多输出设备”，并选中
* 勾选当前的音频设备和Blackhole，主设备设置为当前的设备
* 在OBS中的“场景”中添加“音频输出采集”，选择BlackHole 2ch
* 调整好系统音量，声音输出设备改为刚刚创建的“多输出设备”

## 降噪
* 选择“麦克风”的设置，选择滤镜，添加噪声抑制，选择RNNosie，即可消除大多数电流声和底噪
* ![image](https://user-images.githubusercontent.com/42920883/160775285-e1748bb4-b3a7-4e58-bbd0-f2099e32ef11.png)

* 为了避免尖叫或其他大响度的声音影响观众的收听体验，可以添加一个压缩器，调整阈值，就可以这类大音量的声音降低
* 比率：3:1，阈值：-17db，起始时间：10ms，释放：60ms，输出增益：0db，避免来源：无

* 如果使用N卡（20系以上），可以使用NVIDIA Boardcast来进行AI降噪，效果相当出色，显卡支持的情况下推荐使用
* 下载地址：https://www.nvidia.cn/geforce/broadcasting/broadcast-app/
* 根据提示下载安装
* 在Broadcast，在麦克风界面设置正在使用的麦克风，试听降噪的效果
* 在OBS的混音器，选择麦克风的属性，将设备设置为NVIDIA Broadcast


# 个性化直播间 #
* 根据自己的喜好，添加一些其他元素，如摄像头、留言栏、文字说明等
* 咩播云插件：https://kuabo.cn/
  * 填写直播间号，选择需要的插件,复制地址
  * 在OBS中场景中添加浏览器，输入复制的地址，调整大小，即可完成添加
* 添加文字，填写粉丝群号或直播公告
* 如果仍觉得不够用，可在OBS官方论坛寻找第三方插件：https://obsproject.com/forum/

* 在“来源”中移动元素的位置，可以调节元素间的遮盖关系（类似PS的图层）


## 参考链接 ##
* https://www.bilibili.com/video/BV1pL411N7gP?share_source=copy_web
