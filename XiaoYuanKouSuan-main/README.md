# XiaoYuanKouSuan

## 方案三精简 + 自动循环刷PK

### 食用方法：

#### ~~修改一下 `NEXT_BUTTON_COORDINATES` 中你的屏幕对应的三个点坐标即可自动进入下一场~~
### **更新 自动计算坐标，无需手动更改坐标（测试分辨率：900×1600）**

图：
![位置1、2、3](doc/img/小猿口算_三点_批注.png)

## 演示视频(一次启动之后自动) :



https://github.com/user-attachments/assets/04a81070-95c3-46d7-a15f-1b4a3683638e



<a href="doc/demo.mp4">完整视频链接</a>



>本项目仅供学习和研究使用请于24小时内删除。使用本项目所产生的任何后果由使用者自行承担。在使用本项目之前，请确保您已充分了解相关法律法规，并确保您的行为符合所在国家或地区的法律要求。未经授权的情况下，请勿将本项目用于商业用途或其他非法用途。转载使用请标明出处。


- 方案一： 小猿口算, 采用抓包方式获取题目和答案, 通过 adb 模拟滑动操作 （已失效）
- 方案二： 修改答案为“1”（可 0.00s，仅限练习场）[方案二链接](Change_Answer/README.md)
- 方案三： 修改 pk 场 js 文件（仅限 pk 场）在此特别感谢 [Ulua3809](https://github.com/ulua3809)! [方案三链接](Re_js/README.md)
- 其他思路：[Frida Hook](https://github.com/cr4n5/XiaoYuanKouSuan/issues/92)、[解密(1)](https://github.com/cr4n5/XiaoYuanKouSuan/issues/79)、[解密(2)](https://github.com/xmexg/xyks/issues/9)  [解密(2)实现](https://github.com/xmexg/xyks/tree/master/frida/matchV2)


- [安卓模拟器使用教程](README_EMULATOR.md)


## 演示视频 :movie_camera:

https://github.com/user-attachments/assets/e9ccfa25-4bdd-4b43-855c-af4a045dcb00

## 碎碎念 :thought_balloon:

代码有点屎山哈哈哈哈，抛砖引玉, 有问题欢迎提 issue :bug:

## 环境配置 :hammer_and_wrench:

1. 已 root 的安卓设备(lsposed 等) :iphone:
2. python3​ (version >= 3.10) :snake:
3. adb :electric_plug:


## 使用 :smile:

1. 安装依赖

   ```shell
   pip install -r requirements.txt
   ```

2. 配置已 root 设备

   借助 [TrustMeAlready](https://github.com/ViRb3/TrustMeAlready/releases) 模块禁用 app 的 SSL 加密连接以获得始文

3. 配置 adb

   - 有线调试

     **请先用数据线连接脚本端设备与移动端设备**

     打开设置-开发者选项- USB 调试

   - 无线连接

     **请确保脚本端设备与移动端在同一局域网下**

     打开设置-开发者选项-无线调试，并记录界面显示的 IP 地址与端口

     （大部分设备首次需要先把「有线调试步骤中」的 USB 调试打开，并在**有线连接**的前提下，同意随即弹出的**允许 USB 调试吗**窗口以完成对设备的调试授权，建议勾选一律允许该设备调试。完成授权后，即可拔掉数据线进行无线调试。）

     键入指令：

     ```shell
     adb connect ip:port
     # e.g.
     # adb connect 192.168.0.101:5555（下文中，无线调试以此为例）
     
     # 正确返回以下格式
     connected to 192.168.0.101:5555
     ```

   完成上面任一步骤后，键入指令

   ```shell
   adb devices
   ```

   ```shell
   # 正确返回以下格式
   List of devices attached
   98c54df9        device
   ```

   以确认 adb 是否在本机正确配置成功并连接
   
> [!TIP]
>
> **我该选择什么方式？**
>
> 上面方式适用于不同情况：
>
> - 有线调试
>
>   只需要插入数据线，确保完成连接后，按照后续操作即可，一般传参无需变化，但需要**保持有线连接**
>
> - 无线调试
>
>   适用于你想在局域网下任意位置（如您的床上）进行 PK，但一般而言，无线调试状态会在重启后关闭，并且每次启用都会变更端口，因而你每次运行脚本都需要获取新的无线调试 IP 并传入

   

4. 配置设备代理

   通过这种方式，可以将移动设备的网络请求全部转发至脚本端。

   - 确保脚本端设备与移动端设备在同一局域网下

   - 获取脚本端设备 ip 地址

     （下文中，脚本端设备以 Windows 为例，移动端设备以 Xiaomi MIUI 为例）

     键入指令

     ```shell
     ipconfig
     ```

     返回内容中，`无线局域网适配器 WLAN ` 下的 `IPv4`  地址即为脚本端设备 ip 地址。

     例如：`192.168.31.113`（下文以此为例）。

   - 设置移动端网络

     对移动端 WiFi 项进行设置，更改其代理为 `手动`，并键入主机名为上面获取到的 ip 地址（如 192.168.31.113），端口为 `8080`，保存即可。

5. 运行脚本

   运行格式为：

   ```shell
   python main.py -H <host> -P <port> -AI <adb-ip>
   ```

> [!CAUTION]
>
> 有线调试情况下，无需指定 -AI 的参数，仅适用于**无线调试**

绝大部分情况下，直接键入以下即可：

   ```python
   python main.py
   ```

   将默认填充参数 Host 为 0.0.0.0，Port 为 8080 运行，等同于以下：

   ```shell
   python main.py -H 0.0.0.0 -P 8080
   ```

   对于**无线调试**：

   假设您在无线调试页面获取的 IP 为 192.168.0.101:5555，那么您应该运行：

   ```shell
   python main.py -H 0.0.0.0 -P 8080 -AI 192.168.0.101:5555
   ```

   绝大多数情况下，直接键入以下即可：

   ```shell
   python main.py -AI 192.168.0.101:5555
   ```

   将默认填充参数 Host 为 0.0.0.0，Port 为 8080 运行，并与该 IP 设备进行无线调试连接。

   

   正常情况下，您将看到控制台开始输出大量请求日志，那么您应该离成功不远了。

   至此，您可以操作移动端设备给对方来点小小的技术震撼了lol。

6. （方案三）进入设置，清除小猿口算缓存!!!（多多清除缓存!!!，有许多不成功的原因来自于此）
   ![alt text](doc/img/773b1be382d61dfe65f13b421a8e6f3b.png)
   
7. （方案三）pk 场中任意答案都会判定正确（现已更新模拟点击）

## Q&A :question:

- **运行提示 `ADB 未找到，请先安装 ADB 工具`？**

  请检查当前设备是否正确安装 adb 工具，需将 adb 添加至设备环境变量（亦可以修改代码手动指定 adb 路径）。
- **无线调试未找到设备？**

  - 请确认脚本端设备与移动端设备保持在同一局域网下

  - 请确认移动端设备是否对脚本端设备进行过 USB 调试授权

    若在之前未进行过有线调试，需要至少完成有线调试步骤，以进行调试授权


## 贡献者 :heart:

感谢以下贡献者对本项目的支持与付出：

![GitHub 贡献者](https://img.shields.io/github/contributors/cr4n5/XiaoYuanKouSuan)

<!-- CONTRIBUTORS-START -->

<a href="https://github.com/sd0ric4"><img src="https://avatars.githubusercontent.com/u/63280168?v=4&s=100" width="50" height="50" alt="sd0ric4" /></a>
<a href="https://github.com/cr4n5"><img src="https://avatars.githubusercontent.com/u/136036346?v=4&s=100" width="50" height="50" alt="cr4n5" /></a>
<a href="https://github.com/jhy354"><img src="https://avatars.githubusercontent.com/u/33386148?v=4&s=100" width="50" height="50" alt="jhy354" /></a>
<a href="https://github.com/Fumeng24"><img src="https://avatars.githubusercontent.com/u/114860867?v=4&s=100" width="50" height="50" alt="Fumeng24" /></a>
<a href="https://github.com/MultiWolf"><img src="https://avatars.githubusercontent.com/u/104704213?v=4&s=100" width="50" height="50" alt="MultiWolf" /></a>
<a href="https://github.com/ulua3809"><img src="https://avatars.githubusercontent.com/u/63995396?v=4&s=100" width="50" height="50" alt="ulua3809" /></a>
<a href="https://github.com/xiaou61"><img src="https://avatars.githubusercontent.com/u/113765024?v=4&s=100" width="50" height="50" alt="xiaou61" /></a>
<a href="https://github.com/GSQZ"><img src="https://avatars.githubusercontent.com/u/83207347?v=4&s=100" width="50" height="50" alt="GSQZ" /></a>

<!-- CONTRIBUTORS-END -->

## Star History :star:

[![Star History Chart](https://api.star-history.com/svg?repos=cr4n5/XiaoYuanKouSuan&type=Date)](https://star-history.com/#cr4n5/XiaoYuanKouSuan&Date)

