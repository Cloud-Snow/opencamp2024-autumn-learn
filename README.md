# opencamp2024-autumn-learn 
## 概述
- [训练营官网](https://opencamp.cn/os2edu/camp/2024fall)
- [训练营介绍](https://github.com/LearningOS)
- [常见问题](https://opencamp.cn/os2edu/bbs/1382)
- [信息汇总](https://opencamp.cn/os2edu/bbs/1390)

## 课程安排
![课程安排](/src/all/schedule.png)
## 目录
- [第一阶段](#第一阶段-rust--rustlings)
- [第二阶段](#第二阶段-rcore-设计与实现)

---
## 第一阶段 Rust & Rustlings
### 资料
- [Rust 程序设计语言 中文版](https://rustwiki.org/zh-CN/book/title-page.html)
### 仓库
- [rustlings仓库][rustlings]

[rustlings]:https://github.com/LearningOS/rust-rustlings-2024-autumn-Cloud-Snow

### 2024/09/29
#### 进展
77/110
#### 事件
今天观看了训练营的开营启动会，了解到要记录自己的学习情况，因此创建了这个仓库以记录学习日常。

我在今天之前就已经搭建好了rustlings环境，并提前开始rust的学习，目前已经进行到第77题，正在学习smart_pointers。

这里记录下之前遇到的问题，目前已解决

#### 问题1
- 构建rustlings环境时，[rustlings仓库][rustlings] README文档有以下这段话
> 进入clone下来的目录下的`exercises`文件夹，执行`rustlings watch`依次查看完成情况，并依次完成对应的练习。
- 但我根据文档操作后却提示
> /home/{uername}/.cargo/bin/rustlings must be run from the rustlings directory<br>Try cd rustlings/!
- 然而我在仓库中并没有找到`rustlings`目录，后来猛然发现这个仓库的根目录其实就是`rustlings`目录，于是我便在根目录下执行`rustlings watch`，果然成功。

- **总结**：我怀疑README文档有误，不应该进入`exercises`文件夹，应该直接在根目录下执行命令。(该问题已被修复)

#### 问题2
- 训练营网站的[排行榜](https://opencamp.cn/os2edu/camp/2024fall/stage/1?tab=rank)只显示github名字，不显示真实姓名等信息。
- **解决方法：** 完善训练营账号资料信息，填写github账号名字。

### 2024/09/30

#### 进展
81/110

#### 事件
- 帮助同学配置了rustlings环境，同时发现了一个奇怪的问题。
- 下载了Markdown Preview插件，感觉还挺好用。

#### 学习内容
- 学习了智能指针和并发操作，了解到需要修改数据时使用`Arc::new(Mutex::new(...))`，只读数据数据的话用`Arc::new(...)`就行。
- Cow（Clone on Write）是一种智能指针，用于在需要时进行克隆。它可以在借用和拥有之间切换，从而在性能和内存使用之间取得平衡，定义如下：
``` rust
enum Cow<'a, B: ?Sized + 'a>
where
    B: ToOwned,
{
    Borrowed(&'a B),
    Owned(<B as ToOwned>::Owned),
}
```
- **Borrowed**：表示一个借用的引用。
**Owned**：表示一个拥有的值。

#### 问题1
- 今天遇到了一个很奇怪的问题，以下截图中左侧为`tests/cicv.rs`文件，右侧是`exercises/varibles1.rs`。
- 当我只把rustlings添加到vscode工作区后，analyzer可以分析exercises中的文件，却不能分析tests和src中的文件。
> ![0930-1](src/2024_09_30/1.png)
- 当我只把rustlins父目录添加到vscode工作区后，anlyzer又只可以分析tests和src中的文件，而不能分析exercises中的文件。
> ![0930-2](src/2024_09_30/2.png)
- 只有我把两个文件夹都加入工作区，analyzer才能两者都能分析。
> ![0930-2](src/2024_09_30/3.png)
- 但是这样就会导致每次打开工作区，rustlings项目就会被nanalyzer重复分析，问题我已经发布在了[训练营问答网站](https://opencamp.cn/os2edu/bbs/1385)，希望能有解决办法。(目前已解决)

### 2024/10/01
#### 进展
93/110
#### 事件
- 今日<font color=red>国庆</font>，祝祖国母亲75岁生日快乐！ヾ(≧▽≦*)o
#### 学习内容
- 更加细致地了解了rust是如何实现线程间通信
- 了解了宏的基本概念和用法
- 体验了clippy工具的功能
- 学习了如何在rust中进行类型转换

### 2024/10/02
#### 进展
96/110
#### 事件
-  放假了是真学不进啊。ε(┬┬﹏┬┬)3
- 才发现原来这个conversions专题是要我补充自定义类型的from等实现啊，我还在想怎么用上from呢。。。
#### 学习内容
- 下面两组代码是等价的。
``` rust
let age = parts[1]
    .parse::<usize>()
    .map_err(ParsePersonError::ParseInt)?;
```
``` rust
let age = match parts[1].parse::<usize>() {
    Ok(value) => value,
    Err(e) => return Err(ParsePersonError::ParseInt(e)),
};
```
> map_err 方法接受一个闭包或函数作为参数，当 Result 是 Err 时，它会将错误值传递给这个闭包或函数，并返回一个新的 Result，其中包含转换后的错误值。如果 Result 是 Ok，则直接返回 Ok 值

> ? 运算符用于简化错误处理。它的作用是对 Result 或 Option 类型的值进行解包，如果是 Ok 或 Some，则返回内部的值；如果是 Err 或 None，则将错误或空值返回给调用者。
- from_str被parse方法隐式调用。
- try_from与from的区别是，try_from返回类型是Result，from返回类型是Self。
- AsRef 用于将一个类型转换为另一个类型的引用。<br>AsMut 用于将一个类型转换为其内部数据的可变引用。

### 2024/10/03
#### 进展
100/110
#### 事件
- 链表怎么这么难写啊！！！(╯‵□′)╯︵┻━┻
#### 学习内容
- unsafe rust能进行的操作：
    - 解引用裸指针
    - 调用不安全的函数或方法
    - 访问或修改可变静态变量
    - 实现不安全 trait
    - 访问 union 的字段 
- `Box::into_raw` 和 `Box::from_raw` 是两个用于处理 Box 类型的原始指针的方法 。
    - `Box::into_raw` 方法将一个` Box<T>` 转换为一个原始指针 `*mut T`，但不会释放内存。
    - `Box::from_raw` 方法将一个原始指针 `*mut T` 转换回 `Box<T>`。这意味着你重新获得了该内存的所有权，并且 Box 将负责释放它。
- 在 Rust 的构建脚本（`build.rs`）中，`cargo:` 后的属性用于与 Cargo 进行通信，以便在构建过程中设置环境变量、启用特性、指定重新编译条件等。
    - `cargo:rustc-env=VAR_NAME=VALUE`，其中 `VAR_NAME` 是环境变量的名称，`VALUE` 是其值
    - `cargo:rustc-cfg=feature="FEATURE_NAME"`，其中 `FEATURE_NAME` 是要启用的特性名称。
- `#[no_mangle]` 属性用于防止 Rust 对函数名称进行重整，使得函数可以通过其原始名称进行链接和调用。
- `#[link_name = "my_demo_function"]` 属性用于将 `my_demo_function_alias` 链接到 `my_demo_function`，使得它们实际上是同一个函数。
- `NonNull`是 Rust 标准库中的一个指针类型，它保证指针永远不为 null。
    - 使用 `NonNull::new_unchecked` 将一个原始指针转换为 `NonNull` 指针。这个方法是不安全的，因为它假设传入的指针不为 null。
    - 使用 `as_ptr` 方法将 `NonNull`指针转换为原始指针，然后使用 `unsafe` 块解引用它。
    - 使用 `as_ref` 方法将 `NonNull`指针转换为不可变引用，调用时需要 `unsafe` 块。
    - 使用 `as_mut` 方法将 `NonNull`指针转换为可变引用，调用时需要 `unsafe` 块。

### 2024/10/05
#### 进展
104/110
#### 事件
- 昨天摆了一天，今天终于把链表合并解决了...( ＿ ＿)ノ｜
- 之前一直用 `*(pa.unwrap().as_ptr).val` 来访问`NonNull`指向的值，今天才发现，用 `pa.unwrap().as_ref().val` 和 `pa.unwrap().as_mut().val` 会更方便一些。
- 用明白Option和NonNull后还挺好用

#### 学习内容
- `NonNull`是裸指针类型，实现了`Copy trait`，不会发生所有权转移
- `pa.unwrap().as_ref().val` 和 `pb.unwrap().as_ref().val` 是 T 类型的值。如果 T 没有实现 Copy trait，那么在比较这些值时会发生所有权转移，所以要加‘&’使用其引用

### 2024/10/07
#### 进展
105/110
#### 事件
- 完成了二叉搜索树的查询和插入

#### 学习内容
- `Option::as_mut`：将 `Option<T>` 转换为 `Option<&mut T>`，以便对其中的值进行修改
- `cmp` 方法用于比较两个值。它是` Ord trait` 的一部分，返回一个 `Ordering` `枚举，表示两个值之间的顺序关系。Ordering` 枚举有三个变体：`Less`、`Greater` 和 `Equal`，例如：`a.cmp(&b)` 比较 a 和 b 的值，若`a < b`，则返回 `Less`
- 对于变量 `let mut current = &mut node`，
    - 若有 `current = &mut other_node`，则将`current`借用的目标改变，不改变`node`内容；
    - 若有 `current.right = Some(new_node)`，则`current`借用的目标不变，`node`内容改变，其本质是因为rust自动解引用，其等效于`(*current).right = Some(new_node)`

### 2024/10/08
#### 进展
110/110
#### 事件
- 果然还是在学校里效率高，一口气就把剩下的写完了ヾ(≧▽≦*)o
#### 学习内容
- `for &neighbour in &self.adj[node]`
    - `&self.adj[node]：`
    这里的 & 符号用于借用 `self.adj[node]`，即`&Vec<usize>`。
    - `&neighbour`：
    这里的 & 符号用于模式匹配，`&neighbour` 表示解引用`&usize` 类型的元素，并将其值绑定到 `neighbour` 变量上。
- 在rust标准库中只实现了双端队列 `VecDeque` ，并没有单向队列，使用 `use std::collections::VecDeque` 引入
- `HashSet` 是标准库实现的哈希表，使用 `use std::collections::HashSet` 引入
- 在 Rust 中，可以使用标准库中的 `Vec` 来实现栈。`Vec` 提供了高效的 `push` 和 `pop` 操作，非常适合用来实现栈
- 实现了 `Default` 特性的类型可以通过调用 `T::default()` 来获取一个默认值

#### 问题
- algorithm8怎么是用两个队列实现一个栈，感觉好奇怪。
- algorithm9中的 `smallest_child_idx` 函数我怎么没用到？而且 `new_min` 和 `new_max` 感觉也是多余的。此外， `next` 函数我是是通过 `pop` 掉堆顶元素实现的，但这会破坏堆的结构。要想不怕破坏结构又达到题目效果，估计就要引入一个标记数组了。
- algorithm10插入边，我还在想如何避免插入同样的边呢，结果发现根本不测试这个。仔细想了下，这个要搞的话还有个问题，要是插入的边的两个结点相同，但权重不同，是直接无视，还是更新已经插入的边呢？这样考虑的话就比较复杂了，既然本题不考察这个，索性不管了。
---
## 第二阶段 rCore 设计与实现
### 资料
[rCore-Camp-Guide-2024A](https://learningos.cn/rCore-Camp-Guide-2024A/)
[rCore-Tutorial-Book-v3](https://rcore-os.cn/rCore-Tutorial-Book-v3/)
[RISC-V-Reader](http://riscvbook.com/chinese/RISC-V-Reader-Chinese-v1.pdf)
### 寄存器表
![寄存器表](/src/all/risc-v_register.png)
### 仓库
[rCore-Camp-Code-2024A](https://github.com/LearningOS/2024a-rcore-Cloud-Snow)

### 2024/10/09
#### 事件
- 本以为提前写完了rustlings就需要等下一阶段了，没想到下一阶段的仓库也可以提前创建了，意外之喜。
- 克隆实验代码，配置了qemu环境。

#### 学习内容
- 熟悉运行实验的流程
```  
$ git checkout ch1      #进入章节1
$ cd os                 #进入os目录
$ make run LOG=TRACE    #运行本章代码，并将日志级别设为 TRACE
```
- 可以先按下 `Ctrl+A` ，再按下 `X` 来退出 Qemu
- 可以运行 `make debug` 来调试
- 除了 std 之外，Rust 还有一个不需要任何操作系统支持的核心库 core， 它包含了 Rust 语言相当一部分核心机制
- `riscv64gc-unknown-none-elf` 的 CPU 架构是 riscv64gc，厂商是 unknown，操作系统是 none， elf 表示没有标准的运行时库，没有任何系统调用的封装支持，但可以生成 ELF 格式的执行程序。
#### 问题
- 根据指导手册配置好了qemu环境，运行正常。不过实验要求 `Ubuntu18.04/20.04`，我的则是 `Ubuntu22.04`，希望后面不会出现问题。

### 2024/10/10
#### 事件
第二阶段好难，完全看不懂啊o(TヘTo)
#### 学习内容
- `file <filename>` 查看文件格式
- `rust-readobj -h <filename>` 查看文件头信息
- `rust-objdump -S <filename>` 反汇编导出汇编程序
- QEMU有两种运行模式：
    - User mode 模式，即用户态模拟，如 `qemu-riscv64` 程序， 能够模拟不同处理器的用户态指令的执行，并可以直接解析ELF可执行文件， 加载运行那些为不同处理器编译的用户级Linux应用程序。
    - System mode 模式，即系统态模式，如 `qemu-system-riscv64` 程序， 能够模拟一个完整的基于不同CPU的硬件系统，包括处理器、内存及其他外部设备，支持运行完整的操作系统。
- SBI (Supervisor Binary Interface)是 RISC-V 的一种底层规范，RustSBI 是它的一种实现。 操作系统内核与 RustSBI 的关系有点像应用与操作系统内核的关系，后者向前者提供一定的服务。只是SBI提供的服务很少， 比如关机，显示字符串等。

#### 问题
- 运行`qemu-riscv64 target/riscv64gc-unknown-none-elf/debug/os` 会报错
> [1]    8587 segmentation fault (core dumped)  qemu-riscv64 target/riscv64gc-unknown-none-elf/debug/os
139

### 2024/10/11
#### 事件
- 之前还在想这内核代码都已经给好了，怎么跟着教程改文件，今天才知道，原来是新建一个os项目。
- 下载了even better toml插件，给toml文件高亮

#### 学习内容
- RISC-V
    - `"ecall"`：RISC-V 的系统调用指令，处理器会触发一个环境调用异常，通常用于从用户模式切换到内核模式
    - SYSCALL_EXIT：系统调用编号，值为 93，表示退出系统调用。
    - x10 到 x17：这些寄存器通常用于传递系统调用的参数和返回值。
        - x10 (a0)：第一个参数或返回值。
        - x11 (a1)：第二个参数。
        - x12 (a2)：第三个参数。
        - x17 (a7)：系统调用编号。
    - `inlateout("x10") args[0] => ret`：
        - `inlateout`：用于输入和输出，将值传递给寄存器，并从寄存器读取值。
        - `"x10"`：RISC-V 寄存器 x10（也称为 a0）。
        - `args[0]`：将 args[0] 的值传递给寄存器 x10。
        - `ret`：将 x10 的值存储到变量 ret 中。
    - `in("x11") args[1]`
        - `in`： 仅用于输入，将值传递给寄存器
        - 将 `args[1]` 的值传递给寄存器 `x11`
- `Write trait`
    - `Write trait` 是 Rust 标准库中的一个 trait，定义了写入字符串数据的方法。它通常用于实现自定义的输出目标，例如将格式化字符串写入缓冲区、文件或其他数据结构，定义如下
    ``` rust
    pub trait Write {
        fn write_str(&mut self, s: &str) -> fmt::Result;
        fn write_fmt(&mut self, args: fmt::Arguments) -> fmt::Result {
            // 默认实现
            write_str(self, &format!("{}", args))
        }
    }
    ```
    - `write_str`：这是一个必须实现的方法，用于将字符串数据写入目标。
    - `write_fmt`：这是一个可选实现的方法，用于将格式化数据写入目标。它有一个默认实现，调用 `write_str` 方法
- 宏
    - `#[macro_export]`：这个属性标记宏为可导出，使其可以在其他模块中使用。
    - `$fmt: literal`：匹配一个字面量格式字符串
    - `$(, $($arg: tt)+)?`：匹配可选的逗号后跟一个或多个参数（tt 代表任意标记树）。? 表示这个部分是可选的。
    - `$crate::console::print`：调用当前 crate 中的 console 模块的 print 函数。
    - `format_args!` 宏生成格式化参数，供 print 函数使用。
- extern "C"：指定 _start 函数使用 C 语言的调用约定。
- 加载内核程序的命令
```shell
qemu-system-riscv64 \
            -machine virt \
            -nographic \
            -bios $(BOOTLOADER) \
            -device loader,file=$(KERNEL_BIN),addr=$(KERNEL_ENTRY_PA)
```
- `bios $(BOOTLOADER)` 意味着硬件加载了一个 BootLoader 程序，即 RustSBI

- `device loader,file=$(KERNEL_BIN),addr=$(KERNEL_ENTRY_PA)` 表示硬件内存中的特定位置 `$(KERNEL_ENTRY_PA)` 放置了操作系统的二进制代码 `$(KERNEL_BIN)` 。 `$(KERNEL_ENTRY_PA)` 的值
是 `0x80200000` 。
- 把ELF执行文件转成bianary文件
``` shell
 rust-objcopy --binary-architecture=riscv64 target/riscv64gc-unknown-none-elf/release/os --strip-all -O binary target/riscv64gc-unknown-none-elf/release/os.bin
```

#### 问题
- 在自定义 `panic_handler` 后 `cargo build` 报错
> error: unwinding panics are not supported without std

**原因：** 编译器默认使用了展开 `(unwinding)` 的方式处理 panic，而在 `no_std` 环境中，展开是不被支持的。<br>
**解决办法：** 忘记在 `os/.cargo/config` 中配置 target 了(编译器更推荐命名为`config.toml`)
``` rust
# os/.cargo/config
[build]
target = "riscv64gc-unknown-none-elf"
```
- 自己创建的项目与clone下的仓库中的PanicInfo定义结构不一致<br>
**原因：** 自己创建的项目使用的toolchain 是 `nightly-x86_64-unknown-linux-gnu` ，而clone下的仓库使用的toolchain是 `nightly-2024-05-02-x86_64-unknown-linux-gnu` <br>
**解决办法：** 在项目中添加 `rust-toolchain.toml` 配置 `channel = "nightly-2024-05-02"`

### 2024/10/12
#### 事件
- 到了第二章就不太方便自己从头搭建仓库了，还是用clone的仓库比较好，不过第一章自己从头实现还是挺有收获的。
#### 学习内容
- `#![]` 和 `#[]` 都是属性，`#![]` 属性用于整个 crate 或模块，`#[]` 属性用于特定的项
- `SBI_SHUTDOWN`：通过 SBI 接口请求关机，通常用于操作系统内核。
- `SYSCALL_EXIT`：通过系统调用接口请求退出当前用户态程序，通常用于用户态程序
- .bss和.data区别
    - .bss 段（Block Started by Symbol）是可执行文件中的一部分，通常用于存放程序中的未初始化的全局变量或静态变量，不会占用可执行文件的磁盘空间，在程序运行时分配内存并初始化为 0
    - .data 段用于存放初始化的全局变量和静态变量，数据会被写入到可执行文件中，并占用磁盘空间
- ecall和eret由来
> 为了让应用程序获得操作系统的函数服务，采用传统的函数调用方式（即通常的 call 和 ret 指令或指令组合）将会直接绕过硬件的特权级保护检查。为了解决这个问题， RISC-V 提供了新的机器指令：执行环境调用指令（Execution Environment Call，简称 ecall ）和一类执行环境返回（Execution Environment Return，简称 eret ）指令。
- sret 与 eret 的联系与区别
> eret 代表一类执行环境返回指令，而 sret 特指从 Supervisor 模式的执行环境（即 OS 内核）返回的那条指令，也是本书中主要用到的指令。除了 sret 之外， mret 也属于执行环境返回指令，当从 Machine 模式的执行环境返回时使用， RustSBI 会用到这条指令
- ecall:具有用户态到内核态的执行环境切换能力的函数调用指令；
- sret :具有内核态到用户态的执行环境切换能力的函数返回指令。
- azy_static! 宏提供了全局变量的运行时初始化功能
#### 问题
- 配置完toolchain后报错 
> use of unstable library feature 'panic_info_message'<br>

**解决办法** 在 `main.rs` 中添加 `#![feature(panic_info_message)]`

### 2024/10/13
#### 事件
- 粗略阅读[risc-v手册](http://riscvbook.com/chinese/RISC-V-Reader-Chinese-v1.pdf)

#### 学习内容
- 机器模式(M)和监管模式(S)
    - 机器模式(M)用于运行最可信的代码，是一个RISC-V硬件线程可执行的最高特权模式，在 M 模式下运行的硬件线程能完全访问内存、I/O 和底层系统功能
    - 监管模式(S)用于为Linux、FreeBSD和Windows等操作系统提供支持

|级别|编码|名称|
|---|---|---|
|0|00|用户/应用模式 (U, User/Application)|
|1|01|监督模式 (S, Supervisor)|
|2|10|虚拟监督模式 (H, Hypervisor)|
|3|11|机器模式 (M, Machine)|

### 2024/10/14
#### 学习内容
- 应用管理器的实现
```rust
lazy_static! {
    static ref APP_MANAGER: UPSafeCell<AppManager> = unsafe {
        UPSafeCell::new({
            extern "C" {
                fn _num_app();
            }
            let num_app_ptr = _num_app as usize as *const usize;
            let num_app = num_app_ptr.read_volatile();
            let mut app_start: [usize; MAX_APP_NUM + 1] = [0; MAX_APP_NUM + 1];
            let app_start_raw: &[usize] =
                core::slice::from_raw_parts(num_app_ptr.add(1), num_app + 1);
            app_start[..=num_app].copy_from_slice(app_start_raw);
            AppManager {
                num_app,
                current_app: 0,
                app_start,
            }
        })
    };
}
```
- `lazy_static!`：这是一个宏，用于定义惰性初始化的全局静态变量。惰性初始化意味着变量的初始化只会在第一次使用时发生，而不是在程序启动时立即发生。
- `static ref`：定义一个全局静态引用，保证在整个程序生命周期中只会初始化一次
- `UPSafeCell<AppManager>`：这是对 `AppManager` 的一个包装，调用`exclusive_access()`，允许它在 Rust 的安全规则下进行内部的可变性操作。`UPSafeCell` 一般是用于单线程环境的安全封装，防止全局对象 `APP_MANAGER` 被重复获取。
- `unsafe`：Rust 的所有静态全局变量都是不可变的，使用可变全局变量需要使用 unsafe 代码块来绕过 Rust 的编译器安全检查。
- `extern "C" `表示调用一个用 C 语言编写的外部函数 `_num_app()`，其定义于 `link_app.S ` 中，用于解析出应用数量以及各个应用的开头。
- `let num_app_ptr = _num_app as usize as *const usize`;：首先将 `_num_app` 函数的地址转换成一个 usize，然后再将其转换为一个指向 usize 类型的指针。这意味着 `_num_app` 函数实际上指向的是一个存储有应用数量的地址。不能省去as usize，因为_num_app 是一个符号地址，不能直接被解释为指针
- `let num_app = num_app_ptr.read_volatile()`;：通过使用 `read_volatile` 方法读取指针指向的值，即应用的数量。`read_volatile` 保证读取操作不会被编译器优化，从而确保访问硬件相关的内存地址的正确性。
- `let mut app_start: [usize; MAX_APP_NUM + 1] = [0; MAX_APP_NUM + 1];`：定义了一个长度为 MAX_APP_NUM + 1 的数组，用于存储应用程序的起始地址。
- `let app_start_raw: &[usize] = core::slice::from_raw_parts(num_app_ptr.add(1), num_app + 1);：`创建了一个从 num_app_ptr.add(1) 开始、长度为 num_app + 1 的切片。这个操作读取了应用程序的起始地址列表，from_raw_parts 是一个不安全操作，用于从原始指针和长度创建切片。
- `app_start[..=num_app].copy_from_slice(app_start_raw);`：将 app_start_raw 的数据拷贝到 app_start 数组中。这个操作将应用程序的起始地址填入 app_start 数组。
- 数组长度比应用数量多1是因为除了每个应用程序的起始地址外，还需要记录最后一个应用程序的结束地址。
- _num_app 汇编代码如下，相当于一个数组
``` asm
_num_app:
 7    .quad 3
 8    .quad app_0_start
 9    .quad app_1_start
10    .quad app_2_start
11    .quad app_2_end
12
```
- `.quad` 是指 8 字节（64 位）的数据类型

### 2024/10/15
#### 事件
- 配置ci-user
- 学习特权级切换
- 学习risc-v汇编指令
#### 学习内容
- 进入 S 特权级 Trap 的相关 CSR（Control and Status Register，控制与状态寄存器）

|CSR 名|该 CSR 与 Trap 相关的功能|
|-----|----------------------|
|sstatus|SPP 等字段给出 Trap 发生之前 CPU 处在哪个特权级（S/U）等信息|
|sepc|当 Trap 是一个异常的时候，记录 Trap 发生之前执行的最后一条指令的地址|
|scause|描述 Trap 的原因|
|stval|给出 Trap 附加信息|
|stvec|控制 Trap 处理代码的入口地址|
- stevc结构
    - MODE 位于 [1:0]，长度为 2 bits；
    - BASE 位于 [63:2]，长度为 62 bits。
- 当 MODE 字段为 0 的时候， stvec 被设置为 Direct 模式，此时进入 S 模式的 Trap 无论原因如何，处理 Trap 的入口地址都是 `BASE<<2` ， CPU 会跳转到这个地方进行异常处理。本实验只会将 stvec 设置为 Direct 模式。而 stvec 还可以被设置为 Vectored 模式。
- `csrrw rd, csr, rs` 可以将CSR当前的值读到通用寄存器rd中，然后将通用寄存器rs的值写入该CSR。
- `csrrw sp, sscratch, sp` 起到交换sp和sscratch的效果
- sb（Store Byte）：存储 8 位（1 字节）的数据。
- sh（Store Halfword）：存储 16 位（2 字节）的数据。
- sw（Store Word）：存储 32 位（4 字节）的数据。
- sd（Store Doubleword）：存储 64 位（8 字节）的数据。
- .rept 27：重复以下的指令 27 次
- .set n, 5：设置变量 n 为 5

### 2024/10/16
#### 学习内容
- `.align <n>` 将地址2^n^字节对齐
- sscratch 是一个CSR，用于管理模式下存储临时数据

### 2024/10/19
#### 事件
- 最近实验课好多，都没时间学rust了
- 今天学校有线下交流会，可惜有课去不了

#### 学习内容
- `write_volatile`：这是一个不安全的方法，用于向指定的内存地址写入数据，并确保这个写操作不会被编译器优化掉
- `let app_start = unsafe { core::slice::from_raw_parts(num_app_ptr.add(1), num_app + 1) };`
    - `num_app_ptr.add(1)`：这是一个**指针**运算，将 num_app_ptr 指针向前移动一个位置。假设 num_app_ptr 是一个指向 usize 类型的指针，那么 num_app_ptr.add(1) 将指向下一个 usize 类型的位置。
    - `num_app + 1`：这是**切片的长度**。num_app 是一个 usize 类型的变量，表示应用程序的数量。num_app + 1 表示切片的长度为 num_app + 1。
    - `core::slice::from_raw_parts`：这个函数用于从一个原始指针和长度创建一个切片。它是一个不安全的操作，因为它假设指针和长度是有效的

### 2024/10/23
#### 学习内容
- 在 `.find(|id| inner.tasks[*id].task_status == TaskStatus::Ready)` 这段代码中，id 是一个引用（&usize），因为 find 方法会传递迭代器元素的引用给闭包。通过解引用 *id 获取 id 指向的值，并使用它来索引 inner.tasks。这种方式确保了代码的高效性和安全性，避免了不必要的拷贝。
- 在 Rust 中，`drop` 函数用于显式地销毁一个值并释放其资源
- `sstatus.sie`: Supervisor Interrupt Enable（超级模式中断使能位）
    - 设置（写入1）以允许超级模式下的中断。
    - 清除（写入0）以禁用中断。
- `sstatus.spie`: Supervisor Previous Interrupt Enable（超级模式先前中断使能位）
    - 在处理中断之前，保存当前的sstatus.sie状态。
    - 进入中断处理程序时，将sstatus.sie清除，以防止中断嵌套。
    - 中断处理完成后，可以恢复sstatus.sie的值。