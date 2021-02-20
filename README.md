This project is developed using T uya SDK, which enables you to quickly develop branded apps connect ing and cont rolling smart scenarios of many devices.For more inf ormation, please check Tuya Developer Website.
# TuYa-Tem-Hum-WIFI-Controller
# 【涂鸦智能】 基于Arduino的智能除湿工控器

# 项目简介/Project brief
1、基于`Arduino`设计的工业除湿控制器，`WIFI联网`，报警上报，项目理念适用于`大型除湿机`（如滑冰场除湿）； 
* The industrial dehumidification controller based on `Arduino`, `WiFi networking,` alarm reporting, project concept is suitable for `large dehumidifiers` (such as ice rink dehumidification);

2、手机可远程设置开启阈值；
* The mobile phone can set the threshold remotely;

3、市电供电（220V）,可根据需要更换对应的AC-DC模块，如AC110V~DC5V；
* Utility power supply (220 V), the corresponding AC-DC module can be replaced as needed,such as AC110V~DC5V  ...;

4、手机可通过涂鸦app查看当前温湿度； ； 
* The mobile phone can view the current temperature and humidity through the Tuya app;

5、温湿有变化时`自动上报`（温度0.1℃变化、湿度1%变化）；
* `Automatic report` when temperature and humidity change (temperature 0.1 ℃ change, humidity 1% change)
# 项目使用教程/Project usage
## 配套硬件设备/BOM
 ID      |   Name   | Designator | Footprint|Quantity
 -----------| ---------| ---------- |-----------|--------
1|	100nF|	C1,C4	|C0603	|2
2|	CH340N|	U2	|SOP8_150MIL_JX	|1
3|	LED|	WORK1	|LED0603_RED|	1
4	|1k	|R7	|R0603|	1
5	|100nF|	C14,C9,C8	|C0603|	3
6	|22pF	|C13,C11	|C0603|	2
7	|SMD0805B050TF|	U6	|R0805|	1
8	|S8050	|Q2,Q3	|SOT-23|	2
9	|HDR-M-2.54_2x3	|ICSP1	|HDR-M-2.54_2X3	|1
10	|1nF	|C19,C18	|C0603|	2
11	|SHT30-DIS-B	|U3|	DFN-8_L2.5-W2.5-P0.50-BL-EP|	1
12	|NUD3105LT1G|	Q1	|SOT-23-3_L2.9-W1.6-P1.90-LS2.8-BR|	1
13	|RJ-SS-105DM1	|K1|	RELAY-TH_RJ-SS-XXXXMX	|1
14|	100nF|	C12,C2,C16,C7	|C0603|	4
15	|100uF	|C10,C3,C17,C6,C5|	C0603|	5
16|	AMS1117-3.3	U4|	SOT-223-3_L6.5-W3.4-P2.30-LS7.0-BR|	1
17	|220AC~DC5V700MA(3.5W)|	U1	|优信电子220AC~DC5V700MA(3.5W)|	1
18	|CONN-TH_3P-P5.08	|P3 |	CONN-TH_3P-P5.08	|1
19|	10k|	R3,R9,R10,R6,R1,R4,R5	|R0603	|7
20	|1k	|R2	|R0603	|1
21	|4.7k|	R8,R11	|R0603|	2
22|	TS-1185-C-A-B-B	|RESET1,KEY|	SW-SMD_L4.0-W2.9-LS5.0|	2
23	|16MHz	|X1|	HC-49S_L11.4-W4.8|	1
24|	U-F-M5DW-Y-4|	USB1	|MICRO-USB-SMD_MICRO-USB-A12|	1
25	|ATMEGA328P-AU	|U5	|TQFP-32_L7.0-W7.0-P0.80-LS9.0-BL	|1
26|	Tuya-WB3S`WIFI MODULE`	|ADC1	|WB3S|	1


## 涂鸦SDK类型及版本号/Tuya SDK type and version
*  使用Tuya-WB3S WIFI模块/ Using Tuya-WB3S WIFI module
*  MCU_SDK  | v2.5.6

## 如何再现此项目/How to reproduce
1、注册[涂鸦平台](https://auth.tuya.com/)，并创建产品，下载提供的 MCU_SDK；
* Register the [Tuya platform](https://auth.tuya.com/), create products and download the MCU provided_ SDK；
2、按照电路设计或自行修改；[PCB硬件电路开源](https://oshwhub.com/Red_mt/ming-ri-wu-xian-wen-shi-du-zhuan-gan-qi);
* According to the circuit design or self modification;[PCB Open Source](https://oshwhub.com/Red_mt/ming-ri-wu-xian-wen-shi-du-zhuan-gan-qi);
3、烧录程序或程序改些，可以参考[MCU_SDK使用指导文档链接](https://docs.tuya.com/zh/iot/device-development/access-mode-mcu/wifi-general-solution/software-reference-wifi/overview-of-migrating-tuyas-mcu-sdk?id=K9hhi0xr5vll9)
* For the burning program or program modification, please refer to [MCU_SDK Using the tutorial](https://docs.tuya.com/zh/iot/device-development/access-mode-mcu/wifi-general-solution/software-reference-wifi/overview-of-migrating-tuyas-mcu-sdk?id=K9hhi0xr5vll9)
# 项目实例
## DEMO照片

## App截屏

## 实际演示视频链接

项目使用的开源License 是 MIT/The open source license used in the project is MIT
==
