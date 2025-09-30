# 分支编写第一次

# 1.时钟树-铁头山羊
## 1.1时钟数
### 1.1.1分频器、锁相环和复用器
<img src="https://i-blog.csdnimg.cn/direct/9b6b2901f63648c582e2bd4e82609f13.png" />

* 分频器：分频
* 锁相环：乘法
* 复用器：梯形频率选择
## 1.1.2树根-时钟源4个心脏（HSI/HSE、LSI/LSE）
<img src="https://i-blog.csdnimg.cn/direct/686d56ece3e948a593460bc77da1a617.png" />

* 内外时钟：内部时钟使用方便但是精度不高。内部的IC震荡电路没有外部稳定
## 1.1.3树干-系统时钟如何产生（SYSCLK=HSI/HSE、锁相环）
<table>
    <tr>
        <td title=CAN_H"><center><img src="https://i-blog.csdnimg.cn/direct/27305bfa6b254d7d82bc5b8028878ed5.png" /></center >
      <td>
         <td title="CAN_L"><center><img src="https://i-blog.csdnimg.cn/direct/e3de8e4af590401b865fbfa96ba3ebaf.png" /></center >
      </center >
           <td>
</table>

* 主要任务是产生Systick 
## 1.1.3树枝
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7e211f35e256456d9de98d5ad6a9b8ca.png)
* Systick最高频率为72MHz

## 1.2时钟数编程
### 1.2.1时钟数的初始状态
初始状态，即进入main函数前
![初始状态](https://i-blog.csdnimg.cn/direct/3c5d66947d96430e83e9e6389c3b233a.png)
* SystemInit会在启动main之前配置时钟树配置一下如下图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/14405dbdd0ec48218b27a5d5ca25635f.png)



标准库的启动代码
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ed09887922924b30a45b09d5aebe5025.png)

程序会从这里开始，main函数之前；可用；将指定行的代码注释掉
### 1.2.2时钟树的编程接口（会读即可）
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/da2189a17a934e6e85d89927a5c24315.png)
* 很少碰到这些代码
* RCC_开头复位时钟控制器
* 灰色：当前情况下是关闭的
* 绿色：可开关，当前情况下是开启的
* 带颜色表示可开关
* PLL:锁相环

# 2.定时器
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cb1dda6081a94bc58da262648f1b7d50.png)

### 2.1.1定时器类型
* 基本定时器(6-7*APB1)：拥有**定时中断**、**主模式触发DAC**的功能
* 通用计时器(2-5*APB1)：拥有基本定时器全部功能，额外具有==内外时钟源选择== ==输入捕获==、==输出比较==、==编码器接口==、==主从触发模式==等功能
* 高级定时器(1\8*APB2)：拥有通用定时器全部功能，额外具有重复计数器、死区生成、互补输出、刹车输入等功能
 
**ps:STM32F103C8T6定时器资源:TIM1、TIM2、TIM3、TIM4**
### 2.1.2结构
* 分为时基单元、输出比较、输入捕获和从模式控制器
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dfa0d561c12b4046a4decf01a9120173.png)

##### 1.1.2.1.时基单元
5个部分
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/982e9a19fae44a6092e3b3c2f803a539.png)
* 预分频器：将频率较高的时钟来源降频为频率较低的频率
* ARR：定时周期

上计数下计数和中心计数
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2e609fc7141b47a0ad7494927573bc6b.png)
* 时钟来源：
* ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7d81c4c3478d47f19d2dc66d4daa0dd0.png)
* 默认时钟频率为72MHz
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a4bba1aee5964c78a6fe7bac304224d7.png)
###### 1.1.2.1.时器的预加载
* 出现的问题-突破ARR无法逸出
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/90713eed5ce84565bcaa8a8a4d679c97.png)
等此次逸出结束后，下次一的ARR改变
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d2e6fc546e4f463fb7a01b820ca48cb8.png)
##### 1.1.2.2.输出比较（PWM）
* PWM:脉冲宽度调制，一种周期固定，占空比可调的信号通过调节占空比等效地调节信号的输出幅度
* ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7113a322ec2d4f1abffee83d0dbf9816.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cecd94e8253d492db1085ffedabf11f5.png)
八种模式-常用PWM1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bcafbe026bd54fda8104b1722a4f0d47.png)
互补输出：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dcf2b1a81a8a41e1b04a5e2728ed9802.png)
极性选择：是否取反
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7dd6396f1df64abbb7a1e4f9fb7a9bec.png)
##### 1.1.2.3.输入捕获
* 1.输入捕获的基本原理
捕获电压的变化，电压边沿
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/edf039b30cc043c7bd084dd36ca8626a.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b24392f6b3444fdd9f633c7da5c4a31b.png)
交叉引用的目的：一个引脚，可以省引脚


* 2.输入捕获的内部结构
* 3.输入滤波
* 4.边沿检测
* 5.信号选择
* 6.分频器
##### 1.1.2.4.从模式控制器


# 3.定时器结构图
## 3.1基本定时器
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a366763f3dc84d8cbbe3fa1daf4ef3d0.png)
* 预分频器(PSC-16位)：是16位的最大值为65535，即65536分频
* 计数器（16位）：每来一个上升沿计数加一，也是16位的，最大值位65536
* 自动重装寄存器（16位）：存储计数目标的寄存器
* UI(更新中断)：产生的中断信号通往NVIC
* U(更新事件)：不会触发中断，会触发内部其他电路的工作
* 主模式：DAC将U更新时间映射到TRGO的位置，然后接到DAC触发引脚上
* 只能向上计数
## 3.2通用定时器
<img src="https://i-blog.csdnimg.cn/direct/a7d234bd765c4e30be24e3bd9ec8f1d3.png"/>

计数器溢出频率：CK_CNT_OV = CK_CNT / (ARR + 1)
					       = CK_PSC / (PSC + 1) / (ARR + 1)
## 时钟
但是其输出频率最大不得超过72MHz。 
其中FCLK,HCLK,PCLK都称为系统时钟,但区别如下, 
FCLK,提供给CPU内核的时钟信号,CPU的主频就是指这个信号; 
HCLK,提供给高速总线AHB的时钟信号; 
PCLK,提供给低速总线APB的时钟信号;

