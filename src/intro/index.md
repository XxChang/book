# 引言
欢迎阅读嵌入式Rust:一本关于如何在裸机(比如，微处理器)上使用Rust编程语言的入门书籍。

## 嵌入式Rust是为谁准备的
嵌入式Rust是为了那些既想要进行嵌入式编程，又想要使用Rust语言所提供的高级概念和安全保障的人们而准备的(参见[Who Rust Is For](https://doc.rust-lang.org/book/ch00-00-introduction.html))

## 本书范围
这本书的目的是：
+ 让开发者快速上手Rust嵌入式开发，比如，如何设置一个开发环境。
+ 分享那些关于使用Rust进行嵌入式开发的，现存的，最好的实践经验，比如，如何最大程度上地利用好Rust语言的特性去写更正确的嵌入式软件
+ 某种程度下作为工具书，比如，如何在一个项目里将C和Rust混合在一起使用

虽然尽可能地尝试让这本书可以用于大多数场景，但是为了使读者和作者更容易理解，在所有的示例中，这本书都使用了ARM Cortex-M架构。然而，这本书并不需要读者熟悉这个架构，书中会在需要时对这个架构的特定细节进行解释。

## 这本书是为谁准备的

这本书适合那些有一些嵌入式背景或者有Rust背景的人，然而我相信每一个对Rust嵌入式编程好奇的人都能从这本书中获得某些收获。对于那些先前没有任何经验的人，我们建议你读一下“要求和预备知识”部分。从其它资料中获取、补充缺失的知识，这样能提高你的阅读体验。你可以看看“其它资源”部分，以找到你感兴趣的那些主题的资源。

### 要求和预备知识

+ 你可以轻松地使用Rust编程语言，且在一个桌面环境上写过，运行过，调试过Rust应用。你应该也要熟悉[2018 edition]的术语，因为这本书是面向Rust 2018的。

[2018 edition]: https://doc.rust-lang.org/edition-guide/
+ 你可以轻松地使用其它语言，比如C，C++或者Ada，开发和调试嵌入式系统，且熟悉如下的概念：
  + 交叉编译
  + 存储映射的外设（Memory Mapped Peripherals）
  + 中断
  + I2C，SPI，串口等等常见的接口

### 其它资源

如果你不熟悉上面提到的东西或者你对这本书中提到的某个特定主题感兴趣，你也许能从这些资源中找到有用的信息。

|  主题        |   资源    |     描述     |
|--------------|----------|-------------|
| Rust         | [Rust Book](https://doc.rust-lang.org/book/) | 如果你还不能轻松地使用Rust，我们强烈地建议读这本书。|
| Rust, 嵌入式 | [Discovery Book](https://docs.rust-embedded.org/discovery/) | 如果你从来没有做过嵌入式编程，这本书可能是个更好的开始。 |
| Rust, 嵌入式 | [Embedded Rust Bookshelf](https://docs.rust-embedded.org) | 这里你能找到许多Rust's Embedded Working Group提供的额外资源。|
| Rust, 嵌入式 | [Embedonomicon](https://docs.rust-embedded.org/embedonomicon/) | 在Rust中进行嵌入式编程的细节。 |
| Rust, 嵌入式 | [embedded FAQ](https://docs.rust-embedded.org/faq.html) | 关于嵌入式环境下的Rust会遇到的常见问题。|
| Rust, 嵌入式 | [Comprehensive Rust 🦀: Bare Metal](https://google.github.io/comprehensive-rust/bare-metal.html) | 为期一天的裸机Rust开发课程教材。 |
| 中断 | [Interrupt](https://en.wikipedia.org/wiki/Interrupt) | - |
| 存储映射的IO/外设 | [Memory-mapped I/O](https://en.wikipedia.org/wiki/Memory-mapped_I/O) | - |
| SPI, UART, RS232, USB, I2C, TTL | [Stack Exchange about SPI, UART, and other interfaces](https://electronics.stackexchange.com/questions/37814/usart-uart-rs232-usb-spi-i2c-ttl-etc-what-are-all-of-these-and-how-do-th) | - |

### 翻译

这本书是已经被一些慷慨的志愿者们翻译了。如果你想要将你的翻译列在这里，请打开一个PR去添加它。

* [日文](https://tomoyuki-nakabayashi.github.io/book/)
  ([repository](https://github.com/tomoyuki-nakabayashi/book))

* [中文](https://xxchang.github.io/book/)
  ([repository](https://github.com/xxchang/book))

## 如何使用这本书
这本书通常假设你是按顺序阅读的。之后的章节是建立在先前的章节中提到的概念之上的，先前章节可能不会深入一个主题的细节，因为在随后的章节将会再次重温这个主题。
在大多数示例中这本书将使用[STM32F3DISCOVERY]开发板。这个板子是基于ARM Cortex-M架构的，且基本功能与大多数基于这个架构的CPUs功能相似。微处理器的外设和其它实现细节在不同的厂家之间是不同的，甚至来自同一个厂家，不同处理器系列之间也是不同的。
因此我们建议购买[STM32F3DISCOVERY]开发板来尝试这本书中的例子。

[STM32F3DISCOVERY]: http://www.st.com/en/evaluation-tools/stm32f3discovery.html

## 贡献

这本书的工作主要在[这个仓库]里管理，且主要由[resouces team]开发。

[这个仓库]: https://github.com/rust-embedded/book
[resouces team]: https://github.com/rust-embedded/wg#the-resources-team

如果你按着这本书的操作遇到了什么麻烦，或者这本书的一些部分不够清楚，或者很难进行下去，那这本书就是有个bug，这个bug应该被报道给这本书的[the issue tracker] 。

[the issue tracker]: https://github.com/rust-embedded/book/issues/

修改拼写错误和添加新内容的Pull requests非常欢迎！

## 二次使用这个材料

这本书根据以下许可证发布:

* 本书中包含的代码示例和独立的Cargo项目均根据[MIT License]和[Apache License v2.0]发放许可的。
* 本书中包含的文档，图片和表格均根据[CC-BY-SA v4.0]发放许可的。

[MIT License]: https://opensource.org/licenses/MIT
[Apache License v2.0]: http://www.apache.org/licenses/LICENSE-2.0
[CC-BY-SA v4.0]: https://creativecommons.org/licenses/by-sa/4.0/legalcode

总之：如果你想在你的工作中使用我们的文档或者图片，你需要：

+ 提供合适的授信 (i.e. 在你的幻灯片中提到本书，提供相关页面的连接)
+ 提供[CC-BY-SA v4.0]的许可证的链接
+ 指出你是否改变了材料的内容，在同一个许可证下，可以对材料进行任何改变

也请告诉我这本书对你是否有帮助！

