- ![](https://imgs.zhubai.love/7c941154b6ac48cbb2347b4447369a88.png)
- # 介绍
- MicroBlocks 同时支持I2C和SPI通信。在这篇文章中，我们将建立一个从micro:bit到Arduino的I2C链接，并在它们之间进行通信。
  
  ![](https://imgs.zhubai.love/83e88492ebe44cf1b39989aff75c8a2e.png)
- I2C只使用两条电缆，而且非常容易设置。一条电缆SCL提供系统之间的时钟信号。另一条SDA用于双向传输数据。
  设备被指定为主设备或从设备，主设备控制所有的交流。在我们的项目中，micro:bit将是主设备，Arduino将是从设备。
- # 目标
  
  你将按照说明为两个设备布线，在它们之间建立联系并为它们供电。然后我们将看看两个设备实现I2C协议所需的编码。之后，我们将在micro:bit和Arduino之间交换一些信息。
- # 需要的材料
- BBC micro:bit和USB线
- Arduino UNO和USB线
- 某些版本的micro:bit 扩展板 我们使用了KEYESTUDIO KS0308电机扩展板（[https://wiki.keyestudio.com/Ks0308_keyestudio_Motor_Drive_Breakout_Board_for_micro_bit）。](https://www.google.com/url?q=https://wiki.keyestudio.com/Ks0308_keyestudio_Motor_Drive_Breakout_Board_for_micro_bit%25EF%25BC%2589%25E3%2580%2582&sa=D&source=editors&ust=1643122492710654&usg=AOvVaw3ke1A5wRmb9AqhxolNLYGN) 但许多其他暴露出micro:bit的19和20针的板子也可以做得很好。
- 3条M/F跳线（20厘米）。
- 有两个USB接口的LAPTOP
  
  程序下载:
  For micro:bit:  [I2CToArduino microBlocks program](https://www.google.com/url?q=https://drive.google.com/open?id%3D1JTGZluldkveBK4PrJ2kr9DKDFfTTrLTX&sa=D&source=editors&ust=1643122492712357&usg=AOvVaw2lMYuPZtF80q5fMnL6x8Iu)
  For Arduino:   [microbit_I2Cslave_XMIT_RECV.ino](https://www.google.com/url?q=https://drive.google.com/open?id%3D1rSb1n0hmor9huSrrgh7RPqbfpZVrZgkF&sa=D&source=editors&ust=1643122492712907&usg=AOvVaw3GWPgp3p1ZIWf1V8g7RZXB)
  *注意：假定听众熟悉Arduino IDE的使用和将程序加载到Arduino上。*
- # 设置
  
  让我们看看如何将micro:bit和Arduino系统相互连接，以建立I2C连接。
  
  ![](https://imgs.zhubai.love/df51865a34fe4fd9b96bd9a2f030369e.png)
- *重要的一点是。 Micro:bit和Arduino使用不同的工作电压，分别为3.3V和5V。如果没有提供电压转换的分线板，请不要将这两者直接连接在一起。*
  
  除了上面显示的板子之间的连接外，记得使用各自的USB线将它们插入你的LAPTOP或DESKTOP计算机进行编程。
- # 程序
  
  对于我们的项目，我们将需要两套程序：一套用于micro:bit，另一套用于Arduino。让我们来看看这两个程序。
- ## Micro:bit 代码
- ![](https://imgs.zhubai.love/6d47b773e006466397b29016d6a1ce63.png)
  ![](https://imgs.zhubai.love/9e2f6ee98fb84ef2904df5aa4dcdf62a.png)
- Micro:bit代码分为两部分。 按A键代码集使您能够向Arduino发送一个信息。 按钮-按代码集使你能够从Arduino接收一个信息。
  
  在我们研究这两个功能的代码之前，让我们回顾一下microBlocks IDE的I2C相关编码块。
  
  以下代码块构成了I2C功能。
  
  ![](https://imgs.zhubai.love/fef8fc37152348c9aab55cdd8e5e2fd5.png)
- 该模块读取所选I2C设备的寄存器值。返回的值是一个单字节的整数，范围为0-255。
  
  ![](https://imgs.zhubai.love/1cf9891e2c164f7f8fa2c5bdd79c16ef.png)
- 该模块向选定的I2C设备的选定寄存器写入一个值。使用的值必须是0-255范围内的单字节整数。
  ![](https://imgs.zhubai.love/32715b0505124080ada80667bdc125de.png)
- 该块使我们能够从I2C设备中读取多个字节的数据。列表的长度决定了有多少个字节将被交换。I2C有一个最大32字节的内部限制。因此，通过这个块只能交换1到32个字节的数据。列表中的值必须是0-255范围内的整数值。其他的都将被忽略。
  ![](https://imgs.zhubai.love/1091a6f935074a9d8a96b024fb7f1525.png)
- 这个块使我们能够向I2C设备写入多个字节的数据。列表的长度决定了有多少个字节将被交换。I2C有一个最大32字节的内部限制。所以通过这个块只能交换1到32个字节。列表中的值必须是0-255范围内的整数。其他的都将被忽略。 对于我们的项目，我们将只使用最后两个与列表相关的块。
  
  让我们更详细地看一下这两个函数的代码。
- ## **按键A发送**
- ![](https://imgs.zhubai.love/1066c79e4a244ee1a93b888d98acf653.png)
- 所以我们要做的是，当我们按下micro:bit上的按键 A时，我们想让它向Arduino发送我们的信息。
- 我们从按下按键 A 开始，显示一个">A "的LED图案，以表示该功能正在向 Arduino 发送。
  ![](https://imgs.zhubai.love/4c87451d08a74242ba24175db1c05b41.png)
- 在这一部分，我们定义用到的变量。
  
  * **i2cDevNo** 是我们决定用于Arduino从属设备的 I2C 设备号。这个选择是完全任意的，除了前0-7个设备号是由 i2c 保留的。任何8-127都是可以的。
  * **sendBuffer** 变量用于保存我们要发送给Arduino设备的消息文本。在这个例子中，我创建了一个精确的32字节的消息。然而，它可以是任何小于这个数字的东西。
  * **I2CData** 变量一开始是一个空的列表。我们需要一个列表类型的变量，因为我们将用来发送多字节信息的I2C写列表块需要一个列表。
- ![](https://imgs.zhubai.love/18e2e3f19186402783d7a7dc81cad87f.png)
- 由于我们的信息文本是在字符串变量 sendBuffer 中，我们必须将其转换成I2C写列表块所要求的列表格式。
  
  请记住，I2C 只喜欢0-255之间的整数，而对文本字母或标点符号等一无所知。这意味着，我们必须将信息中的每一个字母转换成0-255之间的unicode ASCII值。然后，我们必须将这些字母逐一存储在列表变量I2CData的连续位置。有什么办法能比让微块处理这项繁重的任务更好呢？所以我们使用一个FOR循环块来迭代sendBuffer变量的长度，并将每个字母添加到I2CData列表中。现在，它已经有了适当的格式，可以被发送出去了。
  
  ![](https://imgs.zhubai.love/5413cc63d082427a9ec1e6308891ef09.png)
- 这里是处理发送数据操作积木。我们告诉 microBlocks 采取我们的 I2CData 列表并将其发送到I2C的 i2cDevNo 13。
- 后台的 I2C 库例程将查看  I2CData 列表的可变长度，并计算出有多少个字符要发送。然后库将向Arduino发出信号，有32个字节的数据在路上，并将其发送出去。大量的活动在后台发生，不需要我们的干预或编码。我们必须感谢 microBlocks 为我们提供了如此简单的服务。不过，Arduino方面就不能这么说了，我们一会儿就会看到。
  
  ![](https://imgs.zhubai.love/550f6dbc0d5f4197a59fb58980a897ff.png)
- 我们终于到达了SEND程序的终点。为了确保在这之前所有的事情都已经完成，我们显示了一条小信息，说明我们已经发送的字节数。
  
  好了，这一点也不难啊 除了几个变量的设置和一个从文本到列表的简单转换外，我们在一个命令块中完成了I2C的发送。
  
  现在，让我们来看看接收方面的情况。
- ## **按钮 B 接收**
  
  ![](https://imgs.zhubai.love/5aa56002350d43019411f858c2e18d21.png)
- 我们要完成的是，当我们按下micro:bit上的按键B时，从Arduino接收一个信息。
  
  我们从按键B被按下开始，显示一个LED图案"< A"，以表示该功能正在从ARDUINO接收信息。
  
  ![](https://imgs.zhubai.love/606dd81a3fbb4bfd9843a72f1ffc8495.png)
- 再次，我们必须定义我们的变量。
  * **i2cDevNo** 是我们的 I2C 设备号，我们将从它那里接收。
  * **I2CData** 需要被设置成一个有32个条目的空列表，准备接收Arduino信息。
  
  ![](https://imgs.zhubai.love/b9eaa53c327a490581b4d1a7a2e3dc8e.png)
- 而读数据动作块--我们告诉 microBlocks 从Arduino设备引脚 #13 读取多个字节，并将收到的信息放入 I2CData 列表变量。同样，我们也没有什么可做的，因为 microBlocks 帮我们完成了 I2C 后台细节操作。然而，为了充分了解它，让我们研究一下幕后的情况。I2C库将初始化I2C协议的细节，如速度，并将自己指定为主设备。然后，它将查看 I2CData 列表的大小，并向Arduino设备引脚 #13 发送一个要求32字节数据的REQUEST消息。当数据到达时，它将把它储存在指定的列表中，并关闭 I2C 链接。
  
  ![](https://imgs.zhubai.love/de732e72c7024f26af408a055511f0ed.png)
- 让我们澄清一些设计决定（当然是由我做出的），以简化系统之间的数据交换。
  
  正如我们之前提到的，I2C 的数据交换可以从1个字节到32个字节不等。因此，为了使我们的程序在发送和接收多少字节方面更加灵活，我决定为32字节的最大长度编写代码，并通过在源自Arduino的较短信息的末尾放置一个NEWLINE字符来处理较短信息的接收问题。这样一来，通过在micro:bit上寻找接收到的信息中的NEWLINE字符，我们就可以知道这是信息的结束。并迅速忽略其余的32个字节，如果有的话。请注意，这个设计决定纯粹是我的选择，除了32字节的限制外，它与I2C操作没有任何关系。我们可以很容易地在一个字节一个字节的基础上进行操作，一次处理一个字符的信息交换。或者一次处理任意数量的字节。这是一个很大的代码块，但是没有办法把它分开，因为它由一个FOR循环和一个嵌入式的IF块组成。
  
  我们将详细检查它以了解发生了什么。
  
  首先，我们的I2C数据以整数格式在I2CData列表变量中等待我们，所有的32个字节。现在我们知道了这些，就更容易理解代码。
  
  ![](https://imgs.zhubai.love/48c1e48b96ad43149bdc0aa859c45351.png)
- FOR循环检查I2CData列表的每个字节，并寻找一个NEWLINE值。NEWLINE字节的ASCII十进制值为10。
  
  因此，当我们在检查的字节中发现这一点时，我们做两件事。
  
  ![](https://imgs.zhubai.love/38480ed612fd4d429141abc93c4b18e8.png)
- 首先，我们计算一个局部变量delCount，并在其中存储到缓冲区结束前的剩余字节数。换句话说，由于我们已经到达了信息的末尾，我们计算还有多少额外的字节，如果有的话。这就是delCount变量所存储的内容。因此，NEWLINE之后的任何内容都是我们不感兴趣的，我们将把它处理掉。
- ![](https://imgs.zhubai.love/dbeaca2a57824e0ba73cb30f44e3baef.png)
- 然后我们使用一个以delCount为参数的REPEAT循环来删除I2CData列表中其余的所有字节。通过使用列表中的DELETE LAST ITEM选项，我们不必担心我们在删除过程中的位置。我们只需从列表的末端删除delCount次数，就可以完成我们的清理工作。
  这给我们留下了一个干净的列表，其中只包含传入信息的有效字节。
  在这之后，我们会显示一个简短的信息来通知这个过程的结束。
  为了显示I2CData的列表值，我们使用JOIN ITEMS OF LIST块将其转换为字符串。
  WAIT块显然是为了留出足够的时间来查看和阅读显示的信息。
  RETURN块只是让我们立即退出FOR循环。
  ![](https://imgs.zhubai.love/e66f94d7e20f45f9a7e313f75905f5dc.png)
- ![](https://imgs.zhubai.love/6a5310d60e6242c58b7caff16af7a560.png)
- 然而，如果检查的字节不是NEWLINE值，这意味着我们有一个正常的信息内容，需要保留它。由于列表中的值是整数格式，我们需要使用STRING FROM UNICODE块将其转换为字符串格式，并用这个新的字符串替换列表中的整数值。
  
  ![](https://imgs.zhubai.love/f54f14b201124da59da2f5ab1eebb13a.png)
- 这就把我们带到了FOR循环逻辑的终点。
  
  由于一个传入的消息有可能包含也有可能不包含新线字节，这取决于它的长度，最后一个SAY块负责显示细节，以防我们没有遇到新线字节。
  
  好了，我们已经成功地到达了micro:bit和microBlocks代码的终点。
  
  下面是按下按键B和收到Arduino信息时的程序显示。
  
  ![](https://imgs.zhubai.love/2803c64564a0470c9d03eb340c461324.png)
- 现在我们来看看Arduino 部分的程序。
- ## Arduino 代码:                                                
  
  ![](https://imgs.zhubai.love/559fb1da8a294ecbac3a74eb0a5e12ac.png)
- Arduino方面的代码使用了Arduino IDE中的标准WIRE库。由于Arduino方面不是一个基于块的开发环境，我们不能享受到我们非常喜欢的microBlocks IDE的许多好处。
  
  同样，作为一个设计决定，我已经决定提供一个程序，它将作为一个SLAVE接收器和SLAVE发射器。这将简化事情，使我们只用一个程序工作。此外，如上所述，我还决定实施一个32字节的发送/接收方案。这样做的主要原因是为了消除在测试项目时的多次代码修改。就Arduino方面而言，人们只需改变设备地址，使之在两边都匹配一次。然后对于任何长度的消息交换，在Arduino那边只需要改变一个字符串数组，就可以了。编译并开始!
  
  看一下代码:
- ```
  // I2C Slave Receiver and Transmitter
  // by Turgut Guneysu
  //
  // Used in demo of micro:bit to Arduino I2C comms
  // Receiver will print all characters received.
  // Transmitter will use a 32 byte buffer,
  //  but messages can be shorter by placing a \n at the end.
  
  #include <Wire.h>
  
  void setup() {
   Wire.begin(13);               // join i2c bus with address #13
   Wire.onReceive(receiveEvent); // register event
   Wire.onRequest(requestEvent); // register event
   Serial.begin(9600);           // start serial for output
   Serial.println("Ready...");
  }
  
  void loop() {
  }
  
  void receiveEvent(int howMany) {    // I2C Receive Event Handler        
   char c;
   Serial.println(howMany);
   while (Wire.available()) {
   c = Wire.read();
   Serial.print(c);        
   }
   Serial.print(" / LAST char: ");   // Also print the last char as DEC
   Serial.print(c);
   Serial.print(" = ");
   Serial.println(c,DEC);        
  }
  
  void requestEvent() {               // I2C Transmit event Handler
   char response[32] = "Hello from Arduino.\n";  
   Wire.write(response);             // respond with \n terminated message
  }
  ```
  
  Arduino程序由WIRE库的include语句和四个函数组成。
- **<Wire.h>** 库提供I2C功能
- **setup()**
  这里是I2C WIRE库被初始化为设备13的地方
- 将两个事件声明为函数，以处理I2C的RECEIVE和SEND事件。
  然后以9600波特打开串行端口，以使程序信息显示。
  并在串行控制台显示 "Ready... "信息。
- **loop()
  **这个程序的主循环没有被使用，因为I2C事件是由中断驱动的，由各自的接收和请求事件处理程序处理。
- **receiveEvent( int howMany )
  **这是处理Arduino收到的I2C信息的处理器。
  
  它在串行控制台显示收到的字节数。
  使用Wire.read()函数将收到的字节从I2C读取到c变量中。
  它在串行控制台中打印它收到的每一个字节
  
  此外，它以DECIMAL ASCII格式打印最后一个字节，以便于区分任何终止字符。
- **requestEvent()
  **这是一个处理程序，发送存储在response[ ]数组中的指定信息。
  
  消息的长度可以达到32字节。如果需要一个更短的消息，只需添加一个"/n "字节，它是终止消息的新线字符。
  micro:bit端将寻找这个NEWLINE字节，而忽略缓冲区阵列的其他部分。
  消息是通过Wire.write(response)函数调用发送出去的。
  
  可以看出，在Arduino方面有更多的编码要做，尽管不是那么复杂。这让人对microBlocks IDE的简单性充满向往。
- ## ARDUINO 串口打印信息
  ![](https://imgs.zhubai.love/1f9cff517bcd49b181d4c9612b7d9a9d.png)
-
## 评论

这个项目将是一个很好的团队工作项目。一个小组可以专注于micro:bit方面，另一个小组专注于Arduino方面。另外，每个小组可以就I2C交换做出设计决定，并相互分享，以及合作实施。一个有趣的思考练习是如何加强程序以适应非常大的数据交换。

尽情享受吧!
-
- MicroBlocks 端  [i2cToARDUINO.ubp](/assets/i2cToARDUINO_1672722757727_0.ubp)
  Arduino 端 [microbit_i2cslave_xmit_recv.ino](/assets/microbit_i2cslave_xmit_recv_1672722808607_0.ino) #程序下载