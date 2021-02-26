#### This project is developed using Tuya SDK, which enables you to quickly develop branded apps connecting and controlling smart scenarios of many devices.
#### For more information, please check Tuya Developer Website.
#### :point_right::point_right::point_right:[中文版 Readme-zh.md](https://github.com/Red-mt/TuYa-Tem-Hum-WIFI-Controller/blob/main/README-zh.md)
# TuYa-Tem-Hum-WIFI-Controller_Based on Arduino
Difficulty | :heart::white_heart::white_heart::white_heart::white_heart:
-----------|-----------
 Recommend | :heart::heart::heart::heart::heart:
 ---------------
# Project brief
1、The industrial dehumidification controller based on `Arduino`, `WiFi networking,` alarm reporting, project concept is suitable for `large dehumidifiers` (such as ice rink dehumidification);

2、The mobile phone can set the threshold remotely;

3、Utility power supply (220 V), the corresponding AC-DC module can be replaced as needed,such as AC110V~DC5V  ...;

4、The mobile phone can view the current temperature and humidity through the Tuya app;

5、`Automatic report` when temperature and humidity change (temperature 0.1 ℃ change, humidity 1% change)
# Project usage
## BOM list
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


## Tuya SDK type and version
*  Using Tuya-WB3S WIFI module
*  MCU_SDK  | v2.5.6

## How to reproduce
1、Register the [Tuya platform](https://auth.tuya.com/), create products and download the MCU provided_ SDK；

2、According to the circuit design or self modification;[PCB Open Source](https://oshwhub.com/Red_mt/ming-ri-wu-xian-wen-shi-du-zhuan-gan-qi);

3、For the burning program or program modification, please refer to [MCU_SDK Using the tutorial](https://docs.tuya.com/zh/iot/device-development/access-mode-mcu/wifi-general-solution/software-reference-wifi/overview-of-migrating-tuyas-mcu-sdk?id=K9hhi0xr5vll9)

4、`'#define PRODUCT_KEY'`in`' protocol. H '`, you need to write your own PID (16 character product unique identification generated by Tuya development platform after creating the product)
```C
#define PRODUCT_KEY "vlkr**** past your PID"    //16 character product unique identification generated by Tuya development platform after creating the product

```
5、Folder "Adafruit_ Sht31" is a SHT function library, which needs to be placed in the libraries file directory of Arduino

6、Because the upper and lower limit of humidity is an alarm value, the initial value cannot be defined in the program, otherwise once the power is switched on and restarted, the set value will be lost. My way here is to store the set threshold value in the ROM of MCU (Arduino's EEPROM), so that the process will read once when it comes, and write the set value when it is issued. In this way, the power down will not lose data。like this:
```C
union data
{
  int a;
  byte b[4];
};
data HUMUPPR;
data HUMLOWER;
for (int n = 0; n &lt; 4; n++) //read EEPROM
    HUMUPPR.b[n] = EEPROM.read(n);
  for (int n = 0; n &lt; 4; n++)//read EEPROM,begin with 4
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
/**********dp_download_maxhum_set_handle*******/
HUMUPPR.a = (int)maxhum_set;
  for (int n = 0; n &lt; 4; n++)//write EEPROM,begin with 0
    EEPROM.write(n, HUMUPPR.b[n]);
/**********dp_download_minihum_set_handle*******/
HUMLOWER.a = (int)minihum_set;
 for (int n = 0; n &lt; 4; n++)
    EEPROM.write(n+4, HUMLOWER.b[n]);//write EEPROM,begin with 4
```

7、Problem avoidance
* Unable to enter the distribution network mode: the underlying Library of sht has a delay of 500ms. When I call and read in the loop, it takes me 7S to return to the WiFi module. I can't enter the distribution network mode even if I don't connect to the Internet. Then I change the underlying delay and the problem is solved. (there may be other solutions, which I haven't found yet.)

```C
boolean Adafruit_SHT31::readTempHum(void) {
  uint8_t readbuffer[6];

  writeCommand(SHT31_MEAS_HIGHREP);

  delay(500);/**********When I measured it, I changed it to 100*************/
  Wire.requestFrom(_i2caddr, (uint8_t)6);
  if (Wire.available() != 6) 
    return false;
  for (uint8_t i=0; i&lt;6; i++) {
    readbuffer[i] = Wire.read();
  //  Serial.print("0x"); Serial.println(readbuffer[i], HEX);
  }
```
# Project examples
## Test video
The demo video is already in the file.[Play Video Online](https://oshwhub.com/Red_mt/ming-ri-wu-xian-wen-shi-du-zhuan-gan-qi);
## Demo Picture
![Image of Yaktocat](https://image.lceda.cn/pullimage/iexv85bvTUkbJ16IA4y0KFnxqGfuu7jssvWeOsSe.jpeg)




>## The open source license used in the project is MIT
>



欢迎大家交流点赞
Using git hub for the first time:hugs: I hope you like it:satisfied:
>:thumbsup:


>* Due to the author's limited English level, this document is produced by translation software
