# 关于GPIO接口的建议

<a id="c-zst-pin"></a>
## Pin类型默认是零大小的(C-ZST-PIN)

由HAL暴露的GPIO接口应该为所有接口或者端口上的每一个管脚提供一个专用的零大小类型，从而当所有的管脚分配静态已知时，提供一个零开销抽象。

每个GPIO接口或者端口应该实现一个`split`方法，它返回一个拥有所有管脚的结构体。

案例:

```rust
pub struct PA0;
pub struct PA1;
// ...

pub struct PortA;

impl PortA {
    pub fn split(self) -> PortAPins {
        PortAPins {
            pa0: PA0,
            pa1: PA1,
            // ...
        }
    }
}

pub struct PortAPins {
    pub pa0: PA0,
    pub pa1: PA1,
    // ...
}
```

<a id="c-erased-pin"></a>
## 管脚类型提供方法去擦除管脚和端口(C-ERASED-PIN)

从编译时到运行时，管脚都应该提供可以改变属性的类型擦出方法，允许在应用中有更多的灵活性。

案例:

```rust
/// 端口 A, 管脚 0。
pub struct PA0;

impl PA0 {
    pub fn erase_pin(self) -> PA {
        PA { pin: 0 }
    }
}

/// 端口A上的A管脚。
pub struct PA {
    /// 管脚号。
    pin: u8,
}

impl PA {
    pub fn erase_port(self) -> Pin {
        Pin {
            port: Port::A,
            pin: self.pin,
        }
    }
}

pub struct Pin {
    port: Port,
    pin: u8,
    // (这些字段)
    // (这些字段可以打包以减少内存占用)
}

enum Port {
    A,
    B,
    C,
    D,
}
```

<a id="c-pin-state"></a>
## 管脚状态应该被编码成类型参数 (C-PIN-STATE)

取决于芯片或者芯片系列，管脚可能被配置为具有不同特性的输出或者输入。这个状态应该编码进类型系统中以避免在错误的状态中使用管脚。

另外，也可以用这个方法使用额外的类型参数编码芯片特定的状态(eg. 驱动强度)。

用来改变管脚状态的方法应该被实现成`into_input`和`into_output`方法。

另外，应该提供`with_{input,output}_state`方法，在一个不同的状态中临时配置一个管脚而不是移动它。

应该为每个的管脚类型提供下列的方法(也就是说，已擦除和未擦除的管脚类型应该提供一样的API):

* `pub fn into_input<N: InputState>(self, input: N) -> Pin<N>`
* `pub fn into_output<N: OutputState>(self, output: N) -> Pin<N>`
* ```ignore
  pub fn with_input_state<N: InputState, R>(
      &mut self,
      input: N,
      f: impl FnOnce(&mut PA1<N>) -> R,
  ) -> R
  ```
* ```ignore
  pub fn with_output_state<N: OutputState, R>(
      &mut self,
      output: N,
      f: impl FnOnce(&mut PA1<N>) -> R,
  ) -> R
  ```

管脚状态应该用sealed traits来绑定。HAL的用户不必添加他们自己的状态。这个traits能提供HAL特定的方法，实现管脚状态API需要这些方法。

案例:

```rust
# use std::marker::PhantomData;
mod sealed {
    pub trait Sealed {}
}

pub trait PinState: sealed::Sealed {}
pub trait OutputState: sealed::Sealed {}
pub trait InputState: sealed::Sealed {
    // ...
}

pub struct Output<S: OutputState> {
    _p: PhantomData<S>,
}

impl<S: OutputState> PinState for Output<S> {}
impl<S: OutputState> sealed::Sealed for Output<S> {}

pub struct PushPull;
pub struct OpenDrain;

impl OutputState for PushPull {}
impl OutputState for OpenDrain {}
impl sealed::Sealed for PushPull {}
impl sealed::Sealed for OpenDrain {}

pub struct Input<S: InputState> {
    _p: PhantomData<S>,
}

impl<S: InputState> PinState for Input<S> {}
impl<S: InputState> sealed::Sealed for Input<S> {}

pub struct Floating;
pub struct PullUp;
pub struct PullDown;

impl InputState for Floating {}
impl InputState for PullUp {}
impl InputState for PullDown {}
impl sealed::Sealed for Floating {}
impl sealed::Sealed for PullUp {}
impl sealed::Sealed for PullDown {}

pub struct PA1<S: PinState> {
    _p: PhantomData<S>,
}

impl<S: PinState> PA1<S> {
    pub fn into_input<N: InputState>(self, input: N) -> PA1<Input<N>> {
        todo!()
    }

    pub fn into_output<N: OutputState>(self, output: N) -> PA1<Output<N>> {
        todo!()
    }

    pub fn with_input_state<N: InputState, R>(
        &mut self,
        input: N,
        f: impl FnOnce(&mut PA1<N>) -> R,
    ) -> R {
        todo!()
    }

    pub fn with_output_state<N: OutputState, R>(
        &mut self,
        output: N,
        f: impl FnOnce(&mut PA1<N>) -> R,
    ) -> R {
        todo!()
    }
}

// 对于`PA`和`Pin`一样的，对于其它管脚类型来说也是。
```
