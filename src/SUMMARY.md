# Summary

<!--

Definition of the organization of this book is still a work in process.

Refer to https://github.com/rust-embedded/book/issues for
more information and coordination

-->

- [引言](./intro/index.md)
    - [硬件](./intro/hardware.md)
    - [`no_std`](./intro/no-std.md)
    - [工具](./intro/tooling.md)
    - [安装](./intro/install.md)
        - [Linux](./intro/install/linux.md)
        - [MacOS](./intro/install/macos.md)
        - [Windows](./intro/install/windows.md)
        - [验证工具链的安装](./intro/install/verify.md)
- [开始](./start/index.md)
  - [QEMU](./start/qemu.md)
  - [硬件](./start/hardware.md)
  - [存储映射的寄存器](./start/registers.md)
  - [半主机模式](./start/semihosting.md)
  - [运行时恐慌(Panicking)](./start/panicking.md)
  - [异常](./start/exceptions.md)
  - [中断](./start/interrupts.md)
  - [IO](./start/io.md)
- [外设](./peripherals/index.md)
    - [Rust尝鲜](./peripherals/a-first-attempt.md)
    - [借用检查器](./peripherals/borrowck.md)
    - [单例](./peripherals/singletons.md)
- [静态保障(static guarantees)](./static-guarantees/index.md)
    - [类型状态编程](./static-guarantees/typestate-programming.md)
    - [把外设当作状态机](./static-guarantees/state-machines.md)
    - [设计约定](./static-guarantees/design-contracts.md)
    - [零成本抽象](./static-guarantees/zero-cost-abstractions.md)
- [可移植性](./portability/index.md)
- [并发](./concurrency/index.md)
- [容器](./collections/index.md)
- [设计模式](./design-patterns/index.md)
    - [HALs](./design-patterns/hal/index.md)
        - [列表](./design-patterns/hal/checklist.md)
        - [命名](./design-patterns/hal/naming.md)
        - [互操性](./design-patterns/hal/interoperability.md)
        - [可预见性](./design-patterns/hal/predictability.md)
        - [GPIO](./design-patterns/hal/gpio.md)
- [给嵌入式C开发者的贴士](./c-tips/index.md)
    <!-- TODO: Define Sections -->
- [互操性](./interoperability/index.md)
    - [使用C的Rust](./interoperability/c-with-rust.md)
    - [使用Rust的C](./interoperability/rust-with-c.md)
- [没有排序的主题](./unsorted/index.md)
  - [优化: 速度与大小间的博弈](./unsorted/speed-vs-size.md)
  - [执行数学运算](./unsorted/math.md)

---

[附录A: 词汇表](./appendix/glossary.md)
