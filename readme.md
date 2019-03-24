HW04
===
This is the hw04 sample. Please follow the steps below.

# Build the Sample Program

1. Fork this repo to your own github account.

2. Clone the repo that you just forked.

3. Under the hw04 dir, use:

	* `make` to build.

	* `make flash` to flash the binary file into your STM32F4 device.

	* `make clean` to clean the ouput files.

# Build Your Own Program

1. Edit or add files if you need to.

2. Make and run like the steps above.

3. Please avoid using hardware dependent C standard library functions like `printf`, `malloc`, etc.

# HW04 Requirements

1. Please practice to reference the user manuals mentioned in [Lecture 04], and try to use the user button (the blue one on the STM32F4DISCOVERY board).

2. After reset, the device starts to wait until the user button has been pressed, and then starts to blink the blue led forever.

3. Try to define a macro function `READ_BIT(addr, bit)` in reg.h for reading the value of the user button.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files, executable files, or binary files)

5. The TAs will clone your repo, build from your source code, and flash to see the result.

[Lecture 04]: http://www.nc.es.ncku.edu.tw/course/embedded/04/

--------------------

- [x] **If you volunteer to give the presentation (demo) next week, check this.**

--------------------

作業要求利用User button來控制藍燈閃爍的開始，首先必須先找出按鈕的連接阜，由UM1472 User manual Discovery kit with STM32F407VG MCU這份文件的第16頁
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/PA0.png)
可以看出User button連接PA0，且PortA也由AHB1控制，因此可藉由RM0090 Reference manual STM32F407的7.3.10找到致能PortA的函式
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/7_3_10.png)
接下來要把PA0設定為輸入訊號，由8.3的table 35可查出要設為輸入需要設定MODER及PUPDR
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/t35.png)
MODER需設為00、PUPDR需設為10（pull-down）
以上步驟程式碼如下圖：
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/step1.png)

接下來要設定READ_BIT
在8.4.5 GPIO port input data register (GPIOx_IDR) (x = A..I/J/K)可找到讀取PA0數值的register，如圖：
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/8_4_5.png)
READ_BIT寫成：
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/read_bit.png)
因此偵測按鈕被按下的程式碼可寫成：
![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/detect.png)
最後將上面函式與藍燈閃爍函式合併可得：

![image](https://github.com/Carl5901/ESEmbedded_HW04/blob/master/image/all.png)

