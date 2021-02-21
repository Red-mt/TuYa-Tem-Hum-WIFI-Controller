#### This project is developed using Tuya SDK, which enables you to quickly develop branded apps connecting and controlling smart scenarios of many devices.
#### For more information, please check Tuya Developer Website.
# 【涂鸦智能】 基于Arduino的智能除湿工控器
难度指数 | :heart::white_heart::white_heart::white_heart::white_heart:
-----------|-----------
推荐指数 | :heart::heart::heart::heart::heart:
# 项目简介/Project brief
1、基于`Arduino`设计的工业除湿控制器，`WIFI联网`，报警上报，项目理念适用于`大型除湿机`（如滑冰场除湿）； 

2、手机可远程设置开启阈值；
3、市电供电（220V）,可根据需要更换对应的AC-DC模块，如AC110V~DC5V；
4、手机可通过涂鸦app查看当前温湿度； ； 
5、温湿有变化时`自动上报`（温度0.1℃变化、湿度1%变化）；

# 项目使用教程/Project usage
## 配套硬件设备/BOM表
序列      |   名称   | 标号 | 封装|数量
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


## 涂鸦SDK类型及版本号
*  使用Tuya-WB3S WIFI模块
*  SDK类型：MCU_SDK  版本号: v2.5.6

## 如何再现此项目
1、注册[涂鸦平台](https://auth.tuya.com/)，并创建产品，下载提供的 MCU_SDK；
2、按照电路设计或自行修改；[PCB硬件电路开源](https://oshwhub.com/Red_mt/ming-ri-wu-xian-wen-shi-du-zhuan-gan-qi);
3、烧录程序或程序改写，可以参考[MCU_SDK使用指导文档链接](https://docs.tuya.com/zh/iot/device-development/access-mode-mcu/wifi-general-solution/software-reference-wifi/overview-of-migrating-tuyas-mcu-sdk?id=K9hhi0xr5vll9)
4、 `protocol.h`文件中的`#define PRODUCT_KEY`位置  需要写你自己的PID(涂鸦开发平台创建产品后生成的16位字符产品唯一标识）
```C
#define PRODUCT_KEY "vlkr需要写你自己的 your PID"    //开发平台创建产品后生成的16位字符产品唯一标识

```
5、文件夹 Adafruit_SHT31 是SHT函数库，需要放到Arduino 的libraries文件目录下

6、设备的湿度上限下限由于是报警值，不能在程序中定义初值，否则一旦调电重启就会丢失设置的值，我这里是方式是把设定的阈值存储在单片机的ROM中（Arduino的EEPROM）,这样进程到来时读取一次，下发设置值时写入。这样掉电也不会丢失数据了。代码如下：

```C
union data
{
  int a;
  byte b[4];
};
data HUMUPPR;
data HUMLOWER;
for (int n = 0; n &lt; 4; n++) //湿度上限读取EEPROM
    HUMUPPR.b[n] = EEPROM.read(n);
  for (int n = 0; n &lt; 4; n++)//湿度下限读取EEPROM,下限起始地址在4
    HUMLOWER.b[n] = EEPROM.read(n + 4);
```
```C
union data
{
  int a;
  byte b[4];
};
data HUMUPPR;
data HUMLOWER;
/**********放置到上限值设置函数dp_download_maxhum_set_handle中*******/
HUMUPPR.a = (int)maxhum_set;
  for (int n = 0; n &lt; 4; n++)//写入湿度上限到EEPROM,上限起始地址是0
    EEPROM.write(n, HUMUPPR.b[n]);
/**********放置到下限值设置函数dp_download_minihum_set_handle中*******/
HUMLOWER.a = (int)minihum_set;
 for (int n = 0; n &lt; 4; n++)
    EEPROM.write(n+4, HUMLOWER.b[n]);//写入湿度下限到EEPROM,下限起始地址是4
```

7、问题处理/Problem avoidance
* 始终无法进入配网模式：SHT的底层库有500ms延时，在循环里调用读取时，我实测要7s才能返回给wifi模块心跳，始终连不上网，也无法进去配网模式，然后我就改了底层的延迟，问题就解决了;（可能还有其他解决的方式，我暂时还没有发现）

```C
boolean Adafruit_SHT31::readTempHum(void) {
  uint8_t readbuffer[6];

  writeCommand(SHT31_MEAS_HIGHREP);

  delay(500);/**********我把这里改成了100,When I measured it, I changed it to 100*************/
  Wire.requestFrom(_i2caddr, (uint8_t)6);
  if (Wire.available() != 6) 
    return false;
  for (uint8_t i=0; i&lt;6; i++) {
    readbuffer[i] = Wire.read();
  //  Serial.print("0x"); Serial.println(readbuffer[i], HEX);
  }
```
# 项目实例/Project examples
## 实际演示视频链接/Test video
演示视频已经放在文件中。[演示视频链接 ](https://oshwhub.com/Red_mt/ming-ri-wu-xian-wen-shi-du-zhuan-gan-qi);
## 实物照片 App截屏/Demo Picture
![Image of Yaktocat](https://image.lceda.cn/pullimage/iexv85bvTUkbJ16IA4y0KFnxqGfuu7jssvWeOsSe.jpeg)




>#### 项目使用的开源License 是 MIT
欢迎大家交流点赞:hugs: :satisfied:
>:thumbsup:
