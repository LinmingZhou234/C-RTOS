# 总言

**这个工程的模板搭建主要是为了降低大疆机甲大师C型开发板的开发门槛，因为很多人觉得大疆给的官方例程过于繁琐（我也这么觉得），所以我从中抽离出了C型开发板的外设使用模板，希望对赛事兵种的开发有所帮助**

# 硬件设备

* **大疆机甲大师C型开发板：**[**点击了解**](https://www.robomaster.com/zh-CN/products/components/general/development-board-type-c)

# 软件环境

* **Keil5-MDK V5.36 [点击下载](https://img.anfulai.cn/bbs/96992/MDK536.EXE) （使用默认编译器 Compiler5）**
* **Keil5的STM32芯片支持包：[点击下载](https://keilpack.azureedge.net/pack/Keil.STM32F4xx_DFP.2.13.0.pack)**
* **操作系统：FreeRTOS v9.0.0 [点击下载](https://sourceforge.net/projects/freertos/files/FreeRTOS/V9.0.0/FreeRTOSv9.0.0.zip/download)  相关教程 [点击了解](https://doc.embedfire.com/rtos/freertos/zh/latest/index.html)**
* **注意：没有使用STM32CubeMX进行图形化配置，原因很好理解，直接操作源文件库函数，更有易于理解工程的建立**

# 代码简介

* **依据文件名就可以知道其中包含的是什么文件**

# 代码的执行

1. **执行启动文件：startup_stm32f407xx.s  进行CPU的复位以及堆栈，异常向量表，内存等初始化，最后进入用户自定义的主函数 （通常是main函数）**
2. **main函数里进行hal库初始化，系统时钟初始化，开发板硬件初始化等，然后创建FreeRTOS任务，创建成功后开启调度器，自此系统开始实时运行**

# 配置文件说明

* **FreeRTOS的配置：FreeRTOSConfig.h**
* **Hal库的配置：stm32f4xx_hal_conf.h**
* **中断回调函数的重定义：stm32f4xx_it.c**
* **所谓配置，一般就是 开启 / 关闭 对应的宏定义，这样keil5在对源文件进行条件编译的时候，就知道哪些需要编译，哪些不需要**
* **重定义中断回调函数的函数名，必须要与启动文件的中断向量表对应相同**


# FreeRTOS使用说明

* **定义任务句柄**
* **创建任务**
* **定义任务入口函数，在user_task.c  （如果任务很复杂，则建议新建立对应的.c/.h文件）**
* **仿照已有的任务函数编写，非常容易**
* **注意，每个任务的功能函数体必须为无限循环，且没有返回值的函数，循环里必须要有阻塞延时，如 *vTaskDelay(10);*  阻塞延时不同于普通的循环延时，这点要注意，普通的延时请使用我定义好的 *void delay_ticks(uint32_t delay)；***
* **如果你使用了STM32CubeMX图形化配置的FreeRTOS，其函数的使用和我在这个文档所诉的一般不同，因为它对FreeRTOS内核进行了封装CMSIS-OS，我是直接使用的FreeRTOS内核函数**


# 尾言

**代码没有调用静态链接库，你可以轻易地找到所有函数的实现**

作者：大连民族大学 CONE战队 周林明

邮箱：1522019841@qq.com
