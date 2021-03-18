# Test Embed on STM32F407VGTx

## Task description

Create a device on STM32 MCU (Nucleo or Discovery board) to count the time of button pressed. Time should be transmitted via UART to PC (putty or minicom). UART-USB converters can be used. 
Values less than 1s should not be transmitted. Data transmitted in console should look like

```
5sec
2sec
10sec
```

# Installing

We assume you have GCC Toolchain and ST-Link installed on your machine.

## Compiling with GCC ARM

Add the gcc compiler to the *PATH*.

```
export PATH=~/toolchains/gcc-arm-none-eabi-xxx-update/bin:$PATH
```

You may also just set the *GCC_PATH* env variable for make.

```
export GCC_PATH=~/toolchains/gcc-arm-none-eabi-xxx-update/bin
```

Run make in ./LS-STM32-ButtonToUART

```
cd ./LS-STM32-ButtonToUART
make
```

If you see this line, you are all green to flashing the program

```
.../toolchains/gcc-arm-none-eabi-xxx-update/bin//arm-none-eabi-objcopy -O ihex LS-STM32-ButtonToUART.elf LS-STM32-ButtonToUART.hex
```

## Flashing with OpenOCD

Check if the system can see the debugger

```
lsusb
```

You should be able to see this line

```
Bus 001 Device 005: ID 0483:374b STMicroelectronics ST-LINK/V2.1
```

You need a script to tell openocd how to flash the program to a specific MCU. Fortunately, openocd comes with scripts for variety of MCUs. You can see all of them here.

```
ls /usr/share/openocd/scripts
```
Let’s check if openocd can talk to the board.

```
openocd -f board/stm32f4discovery.cfg -c targets -c shutdown
```

The all-in-one flash command is the program function.

```
openocd -f board/stm32f4discovery.cfg -c "program LS-STM32-ButtonToUART.elf verify reset exit"
```
