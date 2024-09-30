# opencamp2024-autumn-learn
- [训练营官网](https://opencamp.cn/os2edu/camp/2024fall)
- [常见问题](https://opencamp.cn/os2edu/bbs/1382)

<center> 课程安排 </center>

![课程安排](https://ssl.cdn.maodouketang.com/FjHMtSlM-PwPAX6ttHeElzZwe07E)
## 第一阶段 Rust & Rustlings
### 资料
- [Rust 程序设计语言 中文版](https://rustwiki.org/zh-CN/book/title-page.html)
### 仓库
- [rustlings仓库][rustlings]

[rustlings]:https://github.com/LearningOS/rust-rustlings-2024-autumn-Cloud-Snow

### 2024/09/29
---
今天观看了训练营的开营启动会，了解到要记录自己的学习情况，因此创建了这个仓库以记录学习日常。

我在今天之前就已经搭建好了rustlings环境，并提前开始rust的学习，目前已经进行到第77题，正在学习smart_pointers。

这里记录下之前遇到的问题，目前已解决

**问题1**
- 构建rustlings环境时，[rustlings仓库][rustlings]README文档有以下这段话
> /home/{uername}/.cargo/bin/rustlings must be run from the rustlings directory<br>
Try `cd rustlings/`!

- 但我根据文档操作后却提示
> 进入clone下来的目录下的`exercises`文件夹，执行`rustlings watch`依次查看完成情况，并依次完成对应的练习。

- 然而我在仓库中并没有找到`rustlings`目录，后来猛然发现这个仓库的根目录其实就是`rustlings`目录，于是我便在根目录下执行`rustlings watch`，果然成功。

- **总结**：我怀疑README文档有误，不应该进入`exercises`文件夹，应该直接在根目录下执行命令。

**问题2**
- 训练营网站的[排行榜](https://opencamp.cn/os2edu/camp/2024fall/stage/1?tab=rank)只显示github名字，不显示真实姓名等信息。
- **解决方法：** 完善训练营账号资料信息，填写github账号名字。