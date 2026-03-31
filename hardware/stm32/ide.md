# 常用开发工具简介

|工具|名称|所属公司|说明|
|-|-|-|-|
|IDE|MDK|Keil|STM32最常用的集成开发环境，简单易用|
|IDE|EWARM|IAR|支持STM32开发，用的人少一些|
|仿真器|DAP|ARM|开源、免驱、带虚拟串口功能、速度快、廉价|
|仿真器|STLINK|ST|支持全面、稳定、廉价|
|仿真器|JLINK|Segger|稳定、高速、价格贵|
|串口调试助手|SSCOM|丁丁|稳定、小巧、简单易用|

# Keil uVision 5 (MDK-ARM)

## 下载安装

获取MDK

- MDK软件下载：https://www.keil.com/download/product/
- 器件支持包下载：https://www.keil.com/dd2/pack/ 
    类似Keil.STM32F1xx_DRP.2.4.1.pack，下载后双击安装即可

注意：

- 安装目录及路径不要有任何中文汉字，且路径越短越好
- 电脑系统名和用户名最好不要有任何中文

## 使用技巧

<details>
<summary>编辑器设置</summary>

![](./img/mdkarm.1.png)

![](./img/mdkarm.2.png)

</details>

<details>
<summary>字体和颜色设置</summary>

![](./img/mdkarm.3.png)

</details>

<details>
<summary>用户关键字设置</summary>

![](./img/mdkarm.4.png)

</details>

<details>
<summary>代码提示&语法检测</summary>

![](./img/mdkarm.5.png)

</details>

<details>
<summary>global.prop文件妙用</summary>

在Keil uVision 5的安装路径下有个文件就是配置文件，可以保存下来，直接粘贴到新环境中去就会把个性化配置迁移过去

\keil\stm32\UV4\global.prop

</details>

<details>
<summary>快速定位函数或变量被定义的地方</summary>

1. 选中该函数或变量 + 鼠标右键 + 选择 Go to Definition of xxx

2. 快捷方式：选中该函数或变量 + F12

![](./img/mdkarm.6.png)

</details>

# STM32CubeMX

## 简介

ST开发的一款图形配置工具，可通过配置自动生成初始化代码。

一个图形配置工具，搭配不同系列的STM32Cube固件包，即可支持不同系列的STM32芯片。

<details>
<summary>关联STM32Cube固件包的方法</summary>

关联成功后是绿色的

![](./img/cubemx.1.png)

</details>

STM32CubeMX 的用户使用手册：https://www.st.com/en/development-tools/stm32cubemx.html#documentation

## 新建STM32CubeMX工程步骤

<details>
<summary>1. 工程初步建立：新建工程，选择芯片型号</summary>

</details>

<details>
<summary>2. 时钟模块配置：设置HSE（外部高速时钟）、LSE（外部低速时钟）、MCO（芯片往外部输出的时钟）</summary>

</details>

<details>
<summary>3. 时钟系统配置：PLL（锁相环）、SYSCLK（系统时钟）、AHB、APB1、APB2等等</summary>

![](./img/cubemx.2.png)

</details>

<details>
<summary>4. GPIO引脚配置：以连接在LED灯的IO为例介绍如何配置</summary>

</details>

<details>
<summary>5. Cortex内核配置：SYS (DEBUG)配置、NVIC（优先级分组，在中断部分讲解）</summary>

</details>

<details>
<summary>6. 生成工程源码：设置工程，MDK等，最后生成代码工程</summary>

<details>
<summary>&nbsp;&nbsp;&nbsp;&nbsp;生成工程源码</summary>

选择Advanced

![](./img/cubemx.3.png)

</details>

<details>
<summary>&nbsp;&nbsp;&nbsp;&nbsp;堆栈大小设置</summary>

![](./img/cubemx.4.png)

</details>

</details>


7. 编写用户程序：在 main.c 文件预留的位置编写代码

