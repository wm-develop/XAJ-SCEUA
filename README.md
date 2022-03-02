2022/2/28
实现SCE-UA算法率定新安江模型

# 下载地址
您可以从Github下载:point_right::point_right:[项目文件](https://github.com/lujiabo98/XAJ-SCEUA)。

如有问题，欢迎交流探讨！ 邮箱：1847096852@qq.com 卢家波
来信请说明博客标题及链接，谢谢。

# 目的

用自己编写的[SCE-UA算法](https://blog.csdn.net/weixin_43012724/article/details/121862991)自动率定自己编写的[三水源新安江模型](https://blog.csdn.net/weixin_43012724/article/details/119712548)，检验SCE-UA算法实用性。本地路径`E:\Research\Practice\XAJ+SCEUA\XAJ+SCEUA`

# 如何使用

1. 下载所有[文件](https://github.com/lujiabo98/XAJ-SCEUA)`https://github.com/lujiabo98/XAJ-SCEUA`
2. 用 VS 2019打开`XAJ+SCEUA.sln`
3. 解决方案下右键选择属性，所有配置，所有平台下，将C++语言标准设为 ISO C++20 标准
4. `Release`和`x64`下，重新生成解决方案
5. 将`E:\Research\Practice\XAJ+SCEUA\XAJ+SCEUA\SCEUA\IOexamples`下的`scein.txt`和`E:\Research\Practice\XAJ+SCEUA\XAJ+SCEUA\XAJ\IOexamples`下的非示例文件粘贴到`E:\Research\Practice\XAJ+SCEUA\XAJ+SCEUA\x64\Release`路径下
6. 点击 本地Windows调试器 ，即可运行SCE-UA自动优化程序
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa35655bbef241cb9370a5f142fed68e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6LWW5Lqm5peg,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

# 技术路线

将新安江模型嵌入SCE-UA算法中，主要在`functn()`函数中实现以下三步：

1. 【前处理】在`functn()`中先把自动生成的参数写入到新安江模型的输入文件中；
2. 【运行模型】再调用新安江模型模型计算出结果；
3. 【后处理】最后计算出NSE，将1-NSE作为`functn()`函数返回值。

# 实现方法

将新安江模型和SCE-UA算法的源代码放在同一个解决方案中，主函数是SCE-UA算法，新安江模型写成函数形式，供`functn()`调用。

在函数`functn()`中添加了三个函数，分别为前处理、运行模型和后处理

## 前处理

`PreProcessing()`函数根据参数模板文件`parameter.tpl`比对待率定参数数组，将优化算法生成的参数数值写入待率定模型的参数输入文件`parameter.txt`中。

## 运行模型

`RunModel()`函数调用新安江模型程序

## 后处理

`PostProcessing()`函数调用`ReadValues()`从待率定模型（在这里指新安江模型）输出结果中读取数据（出口断面流量数据`Q.txt`）；调用`CalculateNSE()`计算纳什效率系数`NSE`；因为SCE-UA算法为最小化算法，因此返回`1-NSE`，这样当`1-NSE`越小时，`NSE`越接近1。