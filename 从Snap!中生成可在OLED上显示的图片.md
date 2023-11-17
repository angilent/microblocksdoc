- 来源：https://wiki.microblocks.fun/snap/snap2mb_img_code
- ## 概述
- 本项目演示如何使用 SNAP 程序准备图像，以便在连接到 micro:bit v2 的 0.96 英寸或 2.42 英寸单色 OLED 显示器上传输和显示图像。 这些显示器支持 128x64 像素的分辨率，并要求以特定方式格式化图像数据。这里使用的 SNAP 程序可处理这种图像转换。
- ![](https://wiki.microblocks.fun/image_blocks_from_snap/oledprojpic.png)
- SNAP 程序会生成 MicroBlocks 代码和数据，以便在 MicroBlocks 中处理图像。生成的代码通过系统剪贴板传输到 MicroBlocks 环境中。
- MicroBlocks 项目使用 OLED128x64(1306-09) 库来控制显示硬件。该库是一个完全用 MicroBlocks 编写的硬件驱动程序。它提供的功能类似于 Adafruit 和其他公司编写的 C++ 库，可通过 Arduino 和其他微控制器驱动类似的显示器。
- **注意：该项目只能在 micro:bit v2 或其他具有足够显示缓冲区内存的电路板上运行；不能在 micro:bit v1 上运行。**
- ## 部件
- 您需要以下部件：
  
  micro:bit v2
  用于将 micro:bit 连接到笔记本电脑的 USB 电缆
  128x64 像素 OLED 显示屏（带 1306 或 1309 控制芯片）
  边缘扩展板
  四至七根跳线，取决于所选的连接类型
  本项目可使用两种显示屏类型：
- 0.96 英寸 128x64 OLED 显示器（仅限 I2C）
- ![](https://wiki.microblocks.fun/image_blocks_from_snap/sdd1306front_75p.png)
- 2.42 英寸 OLED 128x64 显示屏（SPI 或 I2C）
- ![](https://wiki.microblocks.fun/image_blocks_from_snap/sdd1309front_75p.png)
- 有些显示器可配置为 I2C 和 SPI 连接，有些则仅支持 I2C。I2C 所需的连接较少，并可与其他 I2C 设备共享 I2C 引脚，但 SPI 的速度更快。
- **注意：使用 I2C 或 SPI 的显示器默认配置为 SPI。它们需要进行硬件修改才能使用 I2C 连接。不方便进行硬件修改的用户可坚持使用 SPI。**
- 您需要一块 Edge Expander 板来连接 micro:bit 的 I2C 或 SPI 引脚。下面是一些示例：
- ![](https://wiki.microblocks.fun/image_blocks_from_snap/mbedge2.png)
- ![](https://wiki.microblocks.fun/image_blocks_from_snap/mbedge1.png)
- ![](https://wiki.microblocks.fun/image_blocks_from_snap/mbedge3.png)
- 将
- 显示器连接到 micro:bit
- 首先必须使用 Edge Expander 板将显示屏连接到 micro:bit v2。这些电路板上的 I2C 和 SPI 引脚已经分开并贴上标签（示例如下所示）。
-