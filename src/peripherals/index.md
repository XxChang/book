# 外设

## 什么是外设?

大多数微处理器不仅仅有一个CPU，RAM，或者Flash存储器 - 它们还包含被用来与微处理器的外部系统进行交互的硅片部分，通过传感器，电机控制器，或者人机接口比如一个显示器或者键盘直接和间接地与周遭世界交互。这些组件统称为外设。

这些外设很有用，因为它们允许一个开发者将处理工作交给它们来做，避免了必须在软件中处理每件事。就像一个桌面开发者如何将图形处理工作让给一个显卡那样，嵌入式开发者能将一些任务让给外设去做，让CPU可以把时间放在做其它更重要的事上，或者为了省电啥事也不做。

如果你看向来自1970s或者1980s的旧型号的家庭电脑的主板(其实，昨日的桌面PCs与今日的嵌入式系统没太大区别)，你将看到:

* 一个处理器
* 一个RAM芯片
* 一个ROM芯片
* 一个I/O控制器

RAM芯片，ROM芯片和I/O控制器(这个系统中的外设)会通过一系列并行的迹(traces)又被称为一个"总线"被加进处理器中。地址总线搬运地址信息，其用来选择处理器希望跟总线上哪个设备通信，还有一个用来搬运实际数据的数据总线。在我们的嵌入式微控制器中，应用了相同的概念 - 只是所有的东西被打包到一片硅片上。

然而，不像显卡，显卡通常有像是Vulkan，Metal，或者OpenGL这样的一个软件API。外设暴露给微控制器的是一个硬件接口，其被映射到一块存储区域。

## 线性的物理存储空间

在一个微控制器上，随便往一些地址写一些数据，比如 `0x4000_0000` 或者 `0x0000_0000`，可能也是一个完全有效的动作。

在一个桌面系统上，访问内存被MMU，或者内存管理单元紧紧地控制着。这个组件有两个主要责任: 对部分内存加入访问权限(防止一个进程读取或者修改另一个进程的内存)；重映射物理内存的段到软件中使用的虚拟内存范围上。微控制器通常没有一个MMU，反而在软件中只使用真实的物理地址。

虽然32位微控制器有一个从`0x0000_0000`到`0xFFFF_FFFF`的线性的物理地址空间，但是它们通常只使用几百KiB的实际内存。有相当大部分的地址空间保留着。在早期的章节中，我们说到RAM被放置在地址`0x2000_0000`处。如果我们的RAM是64KiB大小(i.e. 最大地址为0xFFFF),那么地址 `0x2000_0000`到`0x2000_FFFF`与我们的RAM有关。当我们写入一个位于地址`0x2000_1234`的变量时，内部发生的是，一些逻辑发现了地址的上部(这个例子里是0x2000)，然后激活RAM，以便能操作地址的下部(这个例子里是0x1234)。在一个Cortex-M上，我们也也会把Flash ROM映射进地址 `0x000_0000` 到地址 `0x0007_FFFF` 上 (如果我们有一个512KiB Flash ROM)。微控制器设计者没有忽略这两个区域间的所有剩余空间，反而将外设的接口映射到这些地址上。最后看起来像这样:

![](../assets/nrf52-memory-map.png)

[Nordic nRF52832 Datasheet (pdf)]

## 存储映射的外设

乍一看，与这些外设交互很简单 - 将正确的数据写入正确的地址。比如，在一个串行端口上发送一个32位字，可以直接把那个32位字写入某个存储地址。串行端口外设然后能自动获取和发出数据。

这些外设的配置工作相似。不是调用一个函数去配置一个外设，而是暴露一块地址空间作为硬件API。向一个SPI频率控制寄存器写入 `0x8000_0000`，SPI端口将会按照每秒8MB的速度发送数据。向同个地址写入 `0x0200_0000`，SPI端口将会按照每秒125KiB的速度发送数据。这些配置寄存器看起来有点像这个:

![](../assets/nrf52-spi-frequency-register.png)

[Nordic nRF52832 Datasheet (pdf)]

这个接口是关于如何与硬件交互的，其与被使用的语言无关，无论这个语言是汇编，C，或者Rust。

[Nordic nRF52832 Datasheet (pdf)]: http://infocenter.nordicsemi.com/pdf/nRF52832_PS_v1.1.pdf
