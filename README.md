# opencamp2024-autumn-learn 
## 概述
- [训练营官网](https://opencamp.cn/os2edu/camp/2024fall)
- [训练营介绍](https://github.com/LearningOS)
- [常见问题](https://opencamp.cn/os2edu/bbs/1382)

**课程安排**
![课程安排](/src/all/schedule.png)

## 第一阶段 Rust & Rustlings
### 资料
- [Rust 程序设计语言 中文版](https://rustwiki.org/zh-CN/book/title-page.html)
### 仓库
- [rustlings仓库][rustlings]

[rustlings]:https://github.com/LearningOS/rust-rustlings-2024-autumn-Cloud-Snow

### 2024/09/29
---
#### 进展
77/110
#### 事件
今天观看了训练营的开营启动会，了解到要记录自己的学习情况，因此创建了这个仓库以记录学习日常。

我在今天之前就已经搭建好了rustlings环境，并提前开始rust的学习，目前已经进行到第77题，正在学习smart_pointers。

这里记录下之前遇到的问题，目前已解决

#### 问题1
- 构建rustlings环境时，[rustlings仓库][rustlings]README文档有以下这段话
> 进入clone下来的目录下的`exercises`文件夹，执行`rustlings watch`依次查看完成情况，并依次完成对应的练习。
- 但我根据文档操作后却提示
> /home/{uername}/.cargo/bin/rustlings must be run from the rustlings directory<br>Try cd rustlings/!
- 然而我在仓库中并没有找到`rustlings`目录，后来猛然发现这个仓库的根目录其实就是`rustlings`目录，于是我便在根目录下执行`rustlings watch`，果然成功。

- **总结**：我怀疑README文档有误，不应该进入`exercises`文件夹，应该直接在根目录下执行命令。

#### 问题2
- 训练营网站的[排行榜](https://opencamp.cn/os2edu/camp/2024fall/stage/1?tab=rank)只显示github名字，不显示真实姓名等信息。
- **解决方法：** 完善训练营账号资料信息，填写github账号名字。

### 2024/09/30
---
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
- 在 Rust 的构建脚本（build.rs）中，`cargo:` 后的属性用于与 Cargo 进行通信，以便在构建过程中设置环境变量、启用特性、指定重新编译条件等。
    - `cargo:rustc-env=VAR_NAME=VALUE`，其中 `VAR_NAME` 是环境变量的名称，`VALUE` 是其值
    - `cargo:rustc-cfg=feature="FEATURE_NAME"`，其中 `FEATURE_NAME` 是要启用的特性名称。
- `#[no_mangle]` 属性用于防止 Rust 对函数名称进行重整，使得函数可以通过其原始名称进行链接和调用。
- `#[link_name = "my_demo_function"]` 属性用于将 `my_demo_function_alias` 链接到 `my_demo_function`，使得它们实际上是同一个函数。
- `NonNull`是 Rust 标准库中的一个指针类型，它保证指针永远不为 null。
    - 使用 `NonNull::new_unchecked` 将一个原始指针转换为 `NonNull` 指针。这个方法是不安全的，因为它假设传入的指针不为 null。
    - 使用 `as_ptr` 方法将 `NonNull`指针转换为原始指针，然后使用 `unsafe` 块解引用它。
    - 使用 `as_ref` 方法将 `NonNull`指针转换为不可变引用，调用时需要 `unsafe` 块。
    - 使用 `as_mut` 方法将 `NonNull`指针转换为可变引用，调用时需要 `unsafe` 块。

### 2024/10/04
沉迷小说，进展无

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