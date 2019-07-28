# Intelligent-Wheelchair

This application is designed to apply to the person who can’t use arms or legs well but
without language barriers. The intelligent wheelchair is based on voice control
and can be operated independently through voice commands. Moreover,
the application has a collision- avoidance and warning system. Wheelchair  also can be control by smartphone APP through
Bluetooth. The feature of this application is front-end processing rather than
cloud computing.

- Introduction
  - Function
  - System Architecture
  - APP UI
- HW/SW Setup
  - Required Hardware
  - Required Software
  - Hardware Connection
- User manual

## Introduction

### Function

Voice control through front-end processing

Communicate with APP 

Collision auto-avoidance and warning system

### System Architecture

![](C:\Users\Yuani\Desktop\图片1.png)

### APP UI

![](C:\Users\Yuani\Desktop\图片2.png)

## HW/SW Setup

### Required Hardware

- 1 DesignWare ARC EM Starter Kit(EMSK)
- 1 WEGASUN-M6(Speech recognition)
- 1 HM-10 Bluetooth
- 1 Pmod AD2: 4-channel 12-bit A/D Converter(AD7991)
- 2 Infrared range sensor
- 1 L298N Two lines of motor driver
- 2 Dc Motor
- 1 Speaker
- 1 Inclination Sensor
- 1 MIC
- 1 SD Card 
- 1 LED Light
- 1 buzzer
- 1 5V Booster module

### Required Software

- embARC Open Software Platform(OSP)
- ARC GNU Tool Chain
- Serial port terminal, such as putty
- HMSoft
- M6SE-IDE

### Hardware Connection

- Connect Bluetooth to J1
- Connect Pmod AD2 to J2
- Connect Infrared range sensor and Inclination Sensor to Pmod AD2
- Connect buzzer and  LED Light to J3
- Connect WEGASUN-M6 to J5
- Connect L298N motor driver to J6
- Connect Speaker and MIC to WEGASUN-M6

## User Manual

### Before Running This Application

In order to open  **UART2** we need to modify following files

- emsk_init.c (board\emsk\common)

```
set_pmod_mux(PM1_UR_UART_0 | PM1_LR_SPI_S	\
				| PM2_I2C_HRI		\
				| PM3_GPIO_AC		\
				| PM4_I2C_GPIO_D	\
				| PM5_UR_SPI_M1 | PM5_LR_SPI_M2	\
				| PM6_UR_GPIO_C | PM6_LR_GPIO_A );
```

And replace following files：
 
[dw_uart_obj.h(board\emsk\drivers)](https://github.com/YuaniLee/Intelligent-Wheelchair/blob/master/dw_uart_obj.h)

[dw_uart_obj.c(board\emsk\drivers)](https://github.com/YuaniLee/Intelligent-Wheelchair/blob/master/dw_uart_obj.c)

### Run This Application

#### Speech recognition config

![WEGASUN-M6](C:\Users\Yuani\Desktop\图片3.jpg)

Connect WEGASUN-M6 to PC and open M6SSE-IDE, set the baud rate ,recognition mode, recognition distance and so on. Set your keywords to recognition.

#### Build and run

|       file       |                                  |
| :--------------: | -------------------------------- |
|      ble.c       | Bluetooth control                |
|       M6.c       | Voice control                    |
|     buzzer.c     | Warning system                   |
| driver_control.c | Control L298N drive motor        |
|      main.c      | Main entry of embARC Application |
|     makefile     | Makefile of embARC Application   |



##### Makefile

- Target options about EMSK and toolchain:

  ```
  BOARD ?= emsk
  TOOLCHAIN ?= gnu
  OLEVEL ?= 02
  BD_VER ?= 22
  CUR_CORE ?= arcem7d
  ```

- DEV options

  ```
  EXT_DEV_LIST += ble/hm1x
  DEV_CSRCDIR +=  $(EMBARC_ROOT)/device/peripheral/adc/ad7991
  DEV_INCDIR +=  $(EMBARC_ROOT)/device/peripheral/adc/ad7991
  ```

- The middleware used in your application:

  ```
   MID_SEL = common
  ```

- Directories of source files and header files:

  ```
  # application source dirs
  APPL_CSRC_DIR = .
  APPL_ASMSRC_DIR = .
  
  # application include dirs
  APPL_INC_DIR = .
  
  # include current project makefile
  COMMON_COMPILE_PREREQUISITES += makefile
  
  ### Options above must be added before include options.mk ###
  # include key embARC build system makefile
  override EMBARC_ROOT := $(strip $(subst \,/,$(EMBARC_ROOT)))
  include $(EMBARC_ROOT)/options/options.mk
  ```
`make run`
