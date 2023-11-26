[TOC]

# ESP32学习总结

## 基于Linux开发环境的搭建

### 出现软件包无法下载的情况时

`有几个软件包无法下载，要不运行 apt-get update 或者加上 --fix-missing 的选项再试试？` sudo apt-get update

### 最后为了方便，直接改用Ubuntu 23.0的IOS镜像(新手，初接触Linux 选择最为简单、省事)

* 安装教程照着走后，发现最后一个内容无法克隆，随即采用VScode的方式去安装工具链
  * 在VScode安装过程中，发现老是提示 `.CalledProcessError: Command '['/usr/bin/python3', '-m', 'venv', '--clear', '--upgrade-deps', '/home/cly/.espressif/python_env/idf5.0_py3.11_env']' returned non-zero exit status 1.` 判断是由Python环境缺失导致

    * 初步尝试，使用 `sudo apt-get install python3-venv`，查找资料，怀疑是缺少python3-venv； 下载完重新安装ESP-IDF ,发现此刻就显示安装完成

    ```C
        <!---->
        Successfully built xmlrunner
        Installing collected packages: xmlrunner, psutil
        Successfully installed psutil-5.9.5 xmlrunner-1.7.7

        Python requirements has been installed
    ```
  * `A fatal error occurred: Failed to connect to ESP32: Timed out waiting for packet header` 烧录代码时，遇到这个问题

    * 解决方法：在界面上出现Connecting...字样时候按住boot按钮即可，等到出现烧录的时候就可以松开了
    * `最后实际解决，是因为我VScode 选择的是串口，而我实际插的是USB -- 我一直以为 它是用USB烧录的`

      * 如果还不行，检查虚拟机，是否选对了串口，可能你的虚拟机并没有识别到你window下的设备，需要你手动让虚拟机去识别一下
    * 打印Hello World,程序烧录后显示乱码---很奇怪，我修改了监视方式  从 UART 改成 JTAG 发现不会乱码了，改回串口也不会乱码了---没有发现为什么

      * 网上有方法说，可能是晶振频率是默认40  板子只有26导致，可以修改这个试试，但是我的VS没办法修改，只有默认40，不知道会不会跟我的板子是S3有关，就是默认就是40M

### Linux下，VScode 无法ALT + 左右，快捷切换

在键盘快捷方式里面，就是左下角设置按钮，进去有个`键盘快捷方式`，进入后，搜索`go forward`，`go back`, 重新定义即可，但是，会提示有其他快捷键用了这种组合，我是选择把它删掉，你也可以把冲突的改掉。

### linux命令之alias

在Linux中，alias命令的功能是设置命令的别名，用以简写命令，提高操作效率。根据参数的不同，该命令可查看已设定的别名，或为命令设置新的别名。对于用户自定义别名，仅当前登录期内有效；也可修改配置文件使其长期有效。

```C
	alias的命令格式如下：

    alias 命令别名=‘命令’

    设置完别名后可以使用alias查看已经设置的别名。

    使用unalias可以取消别名。

    如果你使用一个命令当作另一个命令的别名，可以使用\来执行该命令原本的意思。

    我们通过 alias 命令设置的别名，仅限于在当前的 Shell 中使用，如果系统重启了，那么新设置的别名就失效了。

    如果想让别名永久有效的话，就需要把所有的别名设置方案加入到家目录下的.bashrc文件中，然后输入如下代码：
```

### Linux sambe

### menuconfig

menuconfig是Linux平台用于管理代码工程、模块及功能的实用工具。上至决定某一程序模块是否编译，下到某一行具体的代码是否需要编译以及某个项的值在本次编译时该是什么都可由menuconfig来定义。 menuconfig的使用方式通常是在编译系统之前在系统源代码根目录下执行 make menuconfig 命令从而打开一个图形化配置界面，再通过对各项的值按需配置从而达到影响系统编译结果的目的。

其实简单来说，就是menuconfig是一个图形化界面，底层支撑它的是另外一种，称为"Kconfig"的东西. `Kconfig这种语言是比较冷门的，目前刚接触，暂不学习，大概了解有这种东西，有概念即可。`

简单来说，menuconfig类似于 STM32 的CubeMX，进行一些配置

---

## 改用命令行的方式学习

原因是提示缺少这个文件，但是不知道是哪步操作导致的，原先是可以正常用VScode 中的 ESP-IDF进行编译、烧录 ![Alt text](67f77268064950259538d6843ce746f.png)

`既然有问题，百度了也解决不了，重装ESP环境也不行，那就先跳过，尝试命令行发现可以编译，那就先用命令行的方式学习！`

### 命令行基础操作--拷贝一个例程，跑起来并查看LOG

1. 首先要从根目录下的esp文件拷贝例程出来

   ```C
     cd ~/esp
     cp -r $IDF_PATH/examples/get-started/hello_world .
   ```
2. 配置工程

   * 设置环境变量 `. $HOME/esp/esp-idf/export.sh `
   * 采用 `"alias"－命令别名`   -- 我自己使用的是 get\_idf
   * 设置目标芯片 -- esp32s3 要在对应的目录下，运行`idf.py set-target esp32s3`
   * 有些工程还需要配置一些具体参数 在`menuconfig`配置工具里配置,运行`idf.py menuconfig`
3. 编译工程

   `idf.py build`
4. 烧录代码

   * 只烧录代码

     `idf.py -p /dev/ttyACM0 flash`
   * 烧录代码并打开串口监视器

     `idf.py -p /dev/ttyACM0 flash monitor`
5. 监视输出

   `idf.py -p /dev/ttyACM0 monitor`

   `Ctrl+]` 退出监视器
6. 擦除flash

   `idf.py -p PORT erase-flash`

### 从点灯开始

首先要拷贝一个工程出来使用，具体路径参考上面hello world工程的拷贝

`外设类型  在 examples/peripherals文件夹中`

我这里拷贝的是generic\_gpio工程，首先工程是使用FreeRtos，演示了IO输入输出的使用

具体看源码就行了，过多好像也没啥需要笔记的

`其中GPIO的更换，就需要到 idf.py menuconfig 里面去配置`

> 也可以不用去idf.py menuconfig 里面配置，我后面发现一种直接配置IO的方法
>
> ```c
> // 直接配置IO
> #define LED1_IO 13
> #define LED1_IO_PIN (1ULL << LED1_IO)
> ```

#### 关于vTaskDelay()延时函数的使用

> * 这个函数包含在 `#include "freertos/task.h"`文件中定义
> * 延时1s `vTaskDelay(1000/portTICK_PERIOD_MS)`&#x20;
>
>   ==但是不能去掉portTICK\_PERIOD\_MS，去掉就不准，目前还没找到问题==

#### 点灯直接操作GPIO，没啥可说，比较值得一提是，LEDC->PWM模块

> LED 控制器 (LEDC) 主要用于控制 LED，也可产生 PWM 信号用于其他设备的控制。 该控制器有 8 路通道，可以产生独立的波形来驱动 RGB LED 等设备。

> **LED PWM 控制器可在无需 CPU 干预的情况下自动改变占空比，实现亮度和颜色渐变**

* 也就是说在以后无需直接创建定时器，再去操作什么高低电平，直接全部封装完整
* ==具体概念解释==

  1. > (2 \*\* duty\_resolution) - 1 2 的 duty\_resolution次方 - 1
     >
  2. 重新回顾频率、占空比概念：
     频率f = 1 / T(周期)   1秒内有多少个周期即为多少频率
     占空比：高电平占整个周期的比例
     * LEDC 分为软件和硬件控制

       使用 LEDC 基本实例请参照 peripherals/ledc/ledc_basic

       使用 LEDC 改变占空比和渐变控制的实例请参照 peripherals/ledc/ledc_fade

       * `具体内容浏览完官网对应外设章节，主要注意下面几点，其他可以等到实际项目需要再针对去看`

         * 定时器配置 指定 PWM 信号的频率和占空比分辨率。
         * 通道配置 绑定定时器和输出 PWM 信号的 GPIO。
         * 改变 PWM 信号 输出 PWM 信号来驱动LED。可通过软件控制或使用硬件渐变功能来改变 LED 的亮度。
         * 首次 LEDC 配置时，建议先配置定时器（调用函数 ledc\_timer\_config()），再配置通道（调用函数 ledc\_channel\_config()）。这样可以确保 IO 脚上的 PWM 信号自有输出开始其频率就是正确的。
         * ==ESP32-S3所有定时器同用一个时钟源，不支持给不同定时器配置不同的时钟源==

  ### IIC驱动OLED屏幕


  > ESP32有两个I2C通道，任何管脚都可以设置为SDA或SCL。
  >
  > 当时有次用IIC硬件刷后，发现屏幕不亮，没有深究下去，以后要深究下去，而不是草草应付一下。就不管了！
  >
  > 当我记下这笔记的时候，已经是我用第二次移植OLED，这次OLED顺利点亮，也用逻辑分析仪抓取数据正常。
  >
  > 使用方法和注意问题在这总结一下。
  >

  #### 数据手册总结

  > * POR 是 Power-On Reset（上电复位）的缩写，它是指在设备上电启动时，各个硬件部件被初始化为默认或预设的初始状态。
  >

  #### ESP32 IIC整个数据发送流程

  ```mermaid
  flowchart LR
  新建操作IIC句柄 --> 启动信号 --> 写数据,单个或多个 --> 停止信号 --> IIC发送函数 --> 删除IIC句柄
  ```
  #### ESP32 IIC整个数据读取流程

  ```mermaid
  flowchart LR
  新建操作IIC句柄 --> 启动信号 --> 写数据-地址 --> 读数据,单个或多个 --> 停止信号 --> IIC发送函数 --> 删除IIC句柄
  ```
  #### 具体硬件IIC接口

  ```c
  I2C 配置函数：
  	i2c_param_config();
  I2C 功能安装使能函数：
  	i2c_driver_install();
  创建 I2C 连接函数：
  	i2c_cmd_link_create();
  写启动信号到缓存函数：
  	i2c_master_start(); 
  写一个字节的命令放到到缓存函数：
  	i2c_master_write_byte();
  写停止信号到缓存函数：
  	i2c_master_stop();
  I2C 发送函数：
  	i2c_master_cmd_begin();
  删除 I2C 连接函数：
  	i2c_cmd_link_delete();
  ```
  ### IIC注意事项(补充上述驱动OLED没记录到的)

  #### 笔记背景:

  > 上述OLED驱动，基本只用写就完了，我又移植了一个BMP180的驱动，关于上次笔记没记详细的地方，重新补充一些
  >

  #### IIC的IO配置

  > 今天重新新建一个工程，从零添加，发现可以不用 idf.py menuconfig配置IO，目前我也还不知道怎么用
  >

  ```c
  #define IIC_SDA_IO  13
  #define IIC_SCL_IO  12

  #define I2C_MASTER_SCL_IO           IIC_SCL_IO      			/*!< GPIO number used for I2C master clock */
  #define I2C_MASTER_SDA_IO           IIC_SDA_IO      			/*!< GPIO number used for I2C master data  */
  #define I2C_MASTER_NUM              0                          /*!< I2C master i2c port number, the number of i2c peripheral interfaces available will depend on the chip */
  #define I2C_MASTER_FREQ_HZ          100000                     /*!< I2C master clock frequency */
  #define I2C_MASTER_TX_BUF_DISABLE   0                          /*!< I2C master doesn't need buffer */
  #define I2C_MASTER_RX_BUF_DISABLE   0                          /*!< I2C master doesn't need buffer */
  #define I2C_MASTER_TIMEOUT_MS       200
  ```
  #### 关于读写函数的例程代码

  ==关于驱动函数IIC的基本读写，看笔记的时候没注意到 ----很重要的一点，缓存完数据要用 ==`i2c_master_cmd_begin()`==函数发送数据，折腾死我了！！==

  ```c
  //写一个数据到BMP180
  void BMP_WriteOneByte(uint8_t WriteAddr,uint8_t DataToWrite)
  {
      i2c_cmd_handle_t cmd = i2c_cmd_link_create();			/* 创建一个句柄，分配空间 */

      i2c_master_start(cmd);									/* 发送起始信号 */
      i2c_master_write_byte(cmd, 0xEE, I2C_MASTER_ACK);		/* 发送地址 (是否应答) */
      i2c_master_write_byte(cmd, WriteAddr, I2C_MASTER_ACK);	/* 发送寄存器地址 (是否应答) */
      i2c_master_write_byte(cmd, DataToWrite, I2C_MASTER_ACK);/* 发送数据 (是否应答) */
      i2c_master_stop(cmd);									/* 停止信号 */

  	/* 上诉我理解是 类似把一套流程压到栈里，等流程走完，再按顺序依次发送 */
  	i2c_master_cmd_begin(I2C_MASTER_NUM, cmd, I2C_MASTER_TIMEOUT_MS);

      i2c_cmd_link_delete(cmd);								/* 删除句柄，释放空间 */
  }
  ```
  ```c
  //从BMP180读一个字节数据
  uint8_t BMP_ReadOneByte(uint8_t ReadAddr)
  {
  	uint8_t data = 0;

      i2c_cmd_handle_t cmd = i2c_cmd_link_create();			/* 创建一个句柄，分配空间 */

      i2c_master_start(cmd);									/* 发送起始信号 */
      i2c_master_write_byte(cmd, 0xEE, I2C_MASTER_ACK);		/* 发送地址 (是否应答) */
      i2c_master_write_byte(cmd, ReadAddr, I2C_MASTER_ACK);	/* 发送寄存器地址 (是否应答) */
      i2c_master_start(cmd);
      i2c_master_write_byte(cmd, 0xEF, I2C_MASTER_ACK);
      i2c_master_read_byte(cmd, &data, I2C_MASTER_ACK);		/* 读一个字节数据 */
      i2c_master_stop(cmd);									/* 停止信号 */
  	i2c_master_cmd_begin(I2C_MASTER_NUM, cmd, I2C_MASTER_TIMEOUT_MS);	/* 发送 */

      i2c_cmd_link_delete(cmd);								/* 删除句柄，释放空间 */

  	return data;
  }
  ```
  ### 创建新工程文件

  学习完如何进行DEMO的拷贝与基础烧录，总想开始创建一个基础工程，然后慢慢移植学习，直接用demo学习，有什么需要修改，或者想简单实现点东西，也不是很方便。

  * idf的命令行提供了创建工程的操作

    * 命令：`idf.py create-project`

      * 例子： `idf.py create-project template_prj`  -->  在当前路径创建名为template\_prj的工程
    * 命令：`idf.py create-component test1`

      * 在当前路径下创建一个test1的模块，并且初始化好头文件和源文件(推荐创建一个components文件夹，在里面创建,把各个代码模块整理好，方便后期移植)
  * 直接自己新建一个文件夹放入特定文件(暂时不去深入这个问题，后面又去看到，再来补充)

  ##### 文件工程结构

  这是新建的文件 &#x20;

  > \[工程路径]\template\_prj
  > ├── CMakeLists.txt
  > └── main
  > ├── CMakeLists.txt
  > └── template\_prj.c
  >

  完整的工程结构可能如下：

  > myProject/
  > ├── CMakeLists.txt
  > ├── sdkconfig
  > ├── components/
  > │       ├── component1/
  > │       │   ├── CMakeLists.txt
  > │       │   ├── Kconfig
  > │       │   └── src1.c
  > │       └── component2/
  > │           ├── CMakeLists.txt
  > │           ├── Kconfig
  > │           └── src1.c
  > │           └── include/
  > │               └── component2.h
  > ├── main/
  > │   ├── CMakeLists.txt
  > │   ├── src1.c
  > │   └── src2.c
  > │
  > └── build/
  >

  **每个组件目录都包含一个 CMakeLists.txt 文件，里面会定义一些变量以控制该组件的构建过程，以及其与整个项目的集成**

  ##### 多文件编译

  这就得提到上文说的工程结构中的CMakeLists.txt文件了

  [ESP32-添加多目录的自定义组件\_idf\_component\_register\_NULL\_1969的博客-CSDN博客](https://blog.csdn.net/sinat_36568888/article/details/125428659)

  [构建系统 - ESP32 - — ESP-IDF 编程指南 latest 文档 (espressif.com)](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-guides/build-system.html)

  [ESP-IDF 前端工具 - idf.py - ESP32 - — ESP-IDF 编程指南 latest 文档 (espressif.com)](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-guides/tools/idf-py.html)

  &#x9;以上三个链接，都是讲的比较好的，两个官方，一个别的博主的文章

  主要就是配置CMakeLists.txt

  idf_component_register(SRCS "engine.c"
  INCLUDE_DIRS "include"
  PRIV_REQUIRES spark_plug)

  * `idf_component_register`

    idf提供的注册接口
  * `SRCS`

    注册.c文件
  * `INCLUDE_DIRS`

    声明头文件
  * `PRIV_REQUIRES` 和 `REQUIRES`

    包含组件，可以使用外部组件，一个是私有，一个是公有

  如果某个模块内有多个子目录中包含多个.c文件和.h文件，可以参考

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/d063c8cf6aea4cd2a6d4cf76a771af5d.png "在这里插入图片描述")

  ```
  file(GLOB_RECURSE SOURCES src_a/*.c src_b/*.c)

  set(include_dirs 
      inc_a
      src_a
      src_a/src_c
      )
  idf_component_register(SRCS ${SOURCES}
                      INCLUDE_DIRS ${include_dirs})

  ```
  **`这个linux下bash语法一致，有时间看一下bash语法`**

  ### SPI驱动TFT屏幕

  #### 回顾一下SPI的四种模式

  [简单记录一下spi的四种mode (baidu.com)](https://baijiahao.baidu.com/s?id=1765842274549870146\&wfr=spider\&for=pc)

  ![](https://pics7.baidu.com/feed/9d82d158ccbf6c812a7e497a3f489c3932fa405d.png@f_auto?token=6911140d194acca3a891f401dd3e93bd)

  这里我移植的是

  # Linux问题总结

  ### Ubuntu虚拟机连不上网络，右上角没有网络图标

  首先还原最近的设置，因为我原先是samba服务，映射到windows下，怀疑会不会是这个问题(虽然原先用着好好的)

  还原后，发现问题依旧存在，还原了虚拟网络设置依旧存在，百度了问题，最终在一个帖子里找到答案:

  终端运行下面代码重启网络管理器(nmcli)解决这个问题

  sudo nmcli network off
  sudo nmcli network on
