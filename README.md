# 说明

适用于 Tecplot 360 EX 2025 R2, Version 2025.2.0.81941 (Dec  8 2025) 版本
解决 Windows 下用 Tecplot 打开包含中文路径的 dataset 之后再次点击 load data 会报错的问题
这个问题的表现可以参考[这里](https://ask.csdn.net/questions/9264611)

> 不过我不确定这个题主是不是一样的问题

经过我的研究发现是 Tecplot 会记忆之前打开的文件夹【以UTF-8】
然后在点击 load data 的时候会尝试将它转换为 Windows wstring【Unicode-16】
但是它的 narrow_to_wide 函数默认是从 ANSI【中国是GBK】 转为 wstring
在路径包含中文的时候就会报错

因此我搞了这个补丁，让它能正确按照 UTF-8 格式转换，**经测试 load data 没有问题**
但是因为实现原理，修改的影响面有些大，可能会带来其他的编码问题【虽然我还没有遇到过】

***Use at you own risk!***

# 安装方式

补丁使用 [Delta Patcher](https://github.com/marco-calautti/DeltaPatcher) V3.1.6 创建
使用该工具和本仓库的补丁文件修补 Tecplot 安装目录下的 bin/tpsdkdialogs_io_qt.dll
因为是二进制修补，所以请确保被修补文件的版本一致【我设置了 Delta Patcher 检查 checksum】

这里也附上原始文件的 checksum
名称: tpsdkdialogs_io_qt.dll
SHA256: be7c9fe32272491a4deee348fab6f59f4d1ab314ff5f30b2abe4a04041743292
