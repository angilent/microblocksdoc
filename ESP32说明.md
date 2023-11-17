- ESP32开发板的使用方法
- ---
- 这些说明适用于希望在 ESP32 板上使用 MicroBlocks 并且能够应对所涉及的额外技术挑战的用户。
- Espressif Systems 的[ESP32](http://esp32.net/)将 32 位双核处理器与 WiFi 和蓝牙功能以及一组 IO 引脚结合在一起。基本的 ESP32 板没有很多内置功能（只有一个用户 LED），因此大多数有趣的应用都需要连接外部组件。由于需要接线，因此与具有更多内置功能的开发板相比，基本的 ESP32 开发板不太适合年幼的儿童和初学者。
- # 串口设置
- 在某些系统上，您可能需要安装串口驱动程序。根据您板上的 USB 串行芯片，您可能需要以下之一：
  
  [CH340G驱动](https://github.com/nodemcu/nodemcu-devkit/tree/master/Drivers)
  
  [Silcon Labs CP210x 驱动程序](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)。
  
  在 Linux 上，通过在终端中运行以下命令将您自己添加到“dialout”组：
  
  ```
  sudo usermod -G dialout -a `whoami`
  ```
- ## 安装 MicroBlocks 固件
- 要在 ESP32 开发板上安装 MicroBlocks 固件，请启动 MicroBlocks 并插入开发板。从 MicroBlocks（齿轮）菜单中，选择“在板上安装固件”：
  
  ![](https://wiki.microblocks.fun/howtoimg/updatefirmware.png)
  
  然后选择“ESP32”：
  
  ![](https://wiki.microblocks.fun/howtoimg/selectesp32.png)
  
  加载固件时，您将看到一个进度屏幕：
  
  ![](https://wiki.microblocks.fun/howtoimg/espflashing.png)
  
  安装固件后，应该会出现一个绿色圆圈，表明电路板已连接。
  
  ![](https://wiki.microblocks.fun/howtoimg/connected.png)
  
  这可能需要几秒钟。在极少数情况下，您可能需要断开并重新连接电路板。
  
  要验证一切正常，请尝试以下操作：
  
  ![](https://wiki.microblocks.fun/howtoimg/setuserledblock.png)
  
  电路板上的 LED 应该亮起，表明电路板已连接。
  
  如果板上的 LED 不亮，可能是 LED 连接到板上的另一个引脚。（不同的 ESP32 板将用户 LED 连接到不同的引脚。）您可以在您的板的数据表中查找 LED 引脚号，但是编写一个 MicroBlocks 程序来为您找到它会更有趣：
  
  ![](https://wiki.microblocks.fun/howtoimg/led_pin_tester.png)
  
  您已准备好编码！
-
- [[2023-01-02]] #修改日期