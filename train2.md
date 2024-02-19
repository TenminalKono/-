---
description: >-
  本次培训将介绍Buck
  Convertor的简要工作原理，大致选型流程，芯片数据手册阅读注意事项，原理图认识与分析，为制作PCB打下理论基础。本次培训开始将进入EDA实操部分，请还未掌握Altium
  Designer基础使用的同学返回培训0或自行寻找相关材料熟悉AD使用全流程。 
  本次培训将以一个实例介绍DC-DC设计的一般流程，该实例与考核所要完成的题目并非完全一致，不同之处请同学们自行思考设计
---

# 机协硬件组培训2——Buck DC-DC设计

视频教程已更新至B站[https://www.bilibili.com/video/BV1Lp421o7hk/](https://www.bilibili.com/video/BV1Lp421o7hk/)

## 1选型流程

| 规格         | 要求                          |
| ---------- | --------------------------- |
| 输入电压(max)  | >26V                        |
| 输出电压(max)  | >=5V                        |
| 最大输出电流     | 5A                          |
| 最低开关频率     | 450kHz                      |
| 保护特性       | 过流保护，过压保护                   |
| 电源通道(模块设计) | 3路（Fixed5V，ADJ可调至12V，低纹波5V） |
|            |                             |

* 最小维持输出电流：负载持续电流150mA，选用最大输出电流为5A的DC-DC×
* 可焊接性：尽量选择较大封装，慎用QFN,BGA等封装
* 可购买性：首选优信→淘宝→立创上能够直接购买到的芯片，价格合理
* 直接问学长学姐（抄作业）

## 2Buck Convertor原理

* 大致原理（无需了解细节）
* 环路与环路面积控制
  * 环路高频
  * 交变电场产生交变磁场
  * 交变磁场感应交变电场
* 原理拓扑与实际电路

## 3芯片数据手册

* 芯片资源
  * [Datesheet芯片数据手册](https://www.ti.com/product/TPS62933?keyMatch=TPS62933)：最重要，每个厂家都会提供，必看
  * Application note：芯片设计和应用的原厂思考，感兴趣可以看，比较硬核
  * EVM评估板：原厂用于测试芯片性能的板子，大多数厂家会提供，大厂EVM板对硬件设计具有极高参考价值
  * EVM User Guide：评估板的使用报告，测试报告，含有EVM的原理图，PCB
  * PSPICE Model：pspice电路仿真模型，作比赛基本用不上，可用于验证
* 芯片手册
  * 特性，原理图
  * 效率-负载电流特性图
  * 引脚定义与引脚封装Pin Configuration and Functions
  * 极限电气特性Specifications
  * 详细说明Detailed Description：特殊功能配置，运行原理，参数设计计算公式等
  * 应用Application and Implementation：参考设计案例，包括参数设计算例，器件选型等
  * 电源建议Power Supply Recommendations
  * PCB布局Layout：参考布局，强烈建议照抄
* WEBENCH Designer的使用

## 4原理图及器件选型

* 输入电容
* 输出电容
* 电感：一体成型电感
* 电阻：0603

#### 5PCB布局

* 滤波电容先大后小的原则进行排布（大电容滤波低频干扰，小电容滤波高频干扰），输出电容器尽量靠近电感
* 所有器件同层放置（过孔会产生寄生电容与寄生电容加了过孔会增加这个环路的电感，导致发生LC振荡。直接的现象就是在SW处产生高振铃，这个高振铃意味着这个环路中，谐振频率的信号分量很强）
* 紧凑布局，使功率路径尽量短
* 电感尽量靠近IC，电感布线布线的铜箔面积不要过大，覆盖焊盘即可
* 注意控制输入环路和输出环路面积
* 保持完整地平面
* 续流二极管靠近IC和电感，靠近GND
* FeedBack网络，分压电阻靠近IC，网络远离功率回路，采样点设置于滤波器件以后
* 按照信号流入→信号流出布局
* 注意物理/机械冲突
* 就近原则
* 紧凑布局

## 6PCB布线

* 分类：功率线和信号线
  * 功率线：过大电流的线（地线和电源线）→铺铜
  * 信号线：除功率线以外的部分→铜线（8-10mil）
* 地线处理：[技术活不靠人品:DC-DC电源如何安排好地线?](https://www.bilibili.com/video/BV1NK411R7bp/)，环路面积控制，单点接地
* 铺铜承流能力预估
  * 对于1OZ铜厚，在常规情况下，20mil能承载1A左右电流大小
  * 0.5OZ铜厚，40mil能承载1A左右电流大小
  * 打孔和铺铜时保持裕量
  * [https://www.eda365.com/portal.php?mod=view\&aid=12](https://www.eda365.com/portal.php?mod=view\&aid=12)
* 铺铜与焊盘的连接方式：十字连接和全连接
* 过孔承流能力预估
  * 过孔15-30mil，2倍，注意内径满足板厂工艺极限
  * [https://www.eda365.com/article-11-1.html](https://www.eda365.com/article-11-1.html)
* 信号线：远离噪声源
* 散热焊盘：[芯片底部散热的封装越来越常见了，PCB焊盘设计要注意什么？](https://www.bilibili.com/video/BV1og4y1k7s1/)，底部打过孔提升散热能力
* 提升功率线路散热方式：开窗
* 扇热焊盘处所的过孔间隙处所有层不能有信号线穿过

## 7工程输出与资料整理

* [输出gerbera文件](https://blog.csdn.net/weixin\_44215265/article/details/104407634)
* 板框要复制一遍放到keepout layer
* 检查元器件是否买齐[InteractiveHtmlBomForAD](https://github.com/lianlian33/InteractiveHtmlBomForAD)
* DRC,FDM
* 打板经验

***

## 参考资料与延伸阅读

{% file src=".gitbook/assets/硬件组培训2.pptx" %}

* [TPS6293x 采用 SOT583 封装的 3.8V 至 30V、2A/3A 同步降压转换器 数据表 (Rev. D)](https://www.ti.com.cn/cn/lit/gpn/tps62933)
* [技术活不靠人品:DC-DC电源如何安排好地线?(强烈推荐观看)](https://www.bilibili.com/video/BV1NK411R7bp/)
* [RK3588主板电源设计规范及PCB布局布线](https://data.altium.com.cn/ui/core/index.html?mode=public\&shareto=#expl-tabl./SHARED/!2aDHFk0vnu29pC2DEPXAR/xIf7A0R9s1oYqYd5/1.%20Altium%20%E5%AE%98%E6%96%B9%E5%91%A8%E6%9C%AB%E7%9B%B4%E6%92%AD%E8%AF%BE%E4%BB%B6/%E3%80%90%E7%AC%AC%208%20%E8%B5%9B%E5%AD%A3%E3%80%912023%20%E5%AE%98%E6%96%B9%E7%9B%B4%E6%92%AD%E8%AF%BE%E4%BB%B6%EF%BC%8823.1.7%20-%2024.1.6%EF%BC%89/8%E5%B1%82RK3588%E4%B8%BB%E6%9D%BFPCB%E8%AE%BE%E8%AE%A1%20-%20%E9%83%91%E6%8C%AF%E5%AE%87/%E7%AC%AC%E4%BA%8C%E8%AF%BE%EF%BC%9A%E4%B8%BB%E6%9D%BF%E7%94%B5%E6%BA%90%E8%AE%BE%E8%AE%A1%E8%A7%84%E8%8C%83%E5%8F%8APCB%E5%B8%83%E5%B1%80%E5%B8%83%E7%BA%BF)
* [Optimizing Transient Response of Internally Compensated dc-dc Converters With Feedforward Capacitor](https://www.ti.com/cn/lit/an/slva289b/slva289b.pdf?ts=1706234275657)
* [AN-2146 Power Design for SDI and other Noise Sensitive Devices](https://www.ti.com/lit/pdf/snoa561)
* [19届智能车硬件培训之主控板设计](https://www.bilibili.com/video/BV1dm4y1G7qd/)

