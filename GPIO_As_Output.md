# GPIO Ports
There are three port groups available in the MCU, Port A, C and D and in each group, number of pins are there.

Each Port pin can be configured as GPIO output and can be used in the application.

# Uses of GPIO
GPIOs are mainly used for connecting LEDs, Relay control, providing status and control signals when connecting other devices or could be used for driving LCDs like OLED in SPI mode where SPI is emulated by GPIO bit banging, etc.

# CH32V003 MCU Pinout

[IMAGE]

# Read Datasheet
I would strongly recommend you to read the datasheet and go through the electrical characteristics section to know more about capability and limitation of GPIO port/pins. Like output current sourcing and sinking capability, etc.

# Note about GPIO D7 / NRST
Please note PD7 is by default is configured as MCU reset pin, you need to configure it as GPIO by configuring it with WCH Link Programmer Utility or in the code.

[IMAGE]

Two options to change

Option 1:
```C++
FLASH_Unlock();
FLASH_EraseOptionBytes();
FLASH_UserOptionByteConfig(OB_STOP_NoRST, OB_STDBY_NoRST, OB_RST_NoEN, OB_PowerON_Start_Mode_USER);
FLASH_Unlock();
```

Option 2:

If your have WCH-Link Utility in your computer. You also can use it to change PD7 mode.

[IMAGE]

# GPIO as Output Example Code for CH32V003
In order to configure any pin as GPIO output, you need to use the following code, let us call that in a function “GPIOConfig” as shown below, this will be the initialization code and need to be called before using the GPIO.

For any configuration, clock for that GPIO port needs to be enabled first.

Three Parameters are there for any GPIO when configuring as output

- GPIO_Pin: this is to define which Pin, for example for D0 it will be GPIO_Pin_0
- GPIO_Mode: to configure if the output will be an open drain (GPIO_Mode_Out_OD) or push-pull (GPIO_Mode_Out_PP)
- GPIO_Speed: to configure how fast you want control the GPIO. There are three options: GPIO_Speed_10MHz, GPIO_Speed_2MHz, GPIO_Speed_50MHz. This basically configures the drive strength of the GPIO internally.

```C++
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0}; //structure variable used for the GPIO configuration

    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to Enable the clock for Port D

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0; // Defines which Pin to configure
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defines Output Type
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defines speed
    GPIO_Init(GPIOD, &GPIO_InitStructure);

}
```

If multiple pins of same port needs to be configured with similar settings, you can write as shown below (GPIO D0 and D1):

```C++
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0}; //structure variable used for the GPIO configuration

    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to Enable the clock for Port D

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_PIn_1; // Defines which Pin to configure
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defines Output Type
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defines speed
    GPIO_Init(GPIOD, &GPIO_InitStructure);

}
```

but, if you have different settings for different GPIO you can do like this (GPIO D0 and D1):

```C++
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0}; //structure variable used for the GPIO configuration

    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to Enable the clock for Port D

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0; // Defines which Pin to configure
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defines Output Type
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defines speed
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1; // Defines which Pin to configure
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD; // Defines Output Type
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_2MHz; // Defines speed
    GPIO_Init(GPIOD, &GPIO_InitStructure);

}
```

And, now suppose if you want to configure different pins of different ports, you can write code like this: (GPIO D0 and C1)

```C++
void GPIO_Config(void)
{
GPIO_InitTypeDef GPIO_InitStructure = {0}; //structure variable used for the GPIO configuration

RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE); // to Enable the clock for Port C
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to Enable the clock for Port D

GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0; // Defines which Pin to configure
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defines Output Type
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defines speed
GPIO_Init(GPIOD, &GPIO_InitStructure);

GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1; // Defines which Pin to configure
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD; // Defines Output Type
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_2MHz; // Defines speed
GPIO_Init(GPIOC, &GPIO_InitStructure);

}
```

Now, let us see which all functions are available for controlling the GPIO pins or port.

if you go through ch32v00x_gpio.h header file, you can see there following function which you will be using for GPIO output operations

Function names are self explainatory.

```C++
void GPIO_SetBits(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin);
void GPIO_ResetBits(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin);
void GPIO_WriteBit(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin, BitAction BitVal);
void GPIO_Write(GPIO_TypeDef *GPIOx, uint16_t PortVal);
void GPIO_PinLockConfig(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin);
```

- GPIO_SetBits is used for setting the Pin High
- GPIO_ResetBits is used for setting the Pin Low
- GPIO_WriteBit is used when you want to SET and REST the pin with the same function
- GPIO_Write function is provided to set complete GPIO port, for example, if you have connected a 16×2 LCD display with 8 bit data port on Port D, you can use this function to set the data port which will set D0-7 pins with a single function call.
- GPIO_PinLockConfig this very interesting and special function, this helps you lock the pin so that accidently in other part of the configuration cannot be changed.

# CH32V003 Blinking LED Example
Now, let us see how we can blink an LED (connected on D0) with a delay of 500mSec so the LED will be ON for 500 mSec and OFF for 500 mSec. This is using GPIO_WriteBit function.

Delay_Ms function is available to give a block delay in milliseconds.

```C++
while(1)
{
    Delay_Ms(500);
    GPIO_WriteBit(GPIOD, GPIO_Pin_0, SET);
    Delay_Ms(500);
    GPIO_WriteBit(GPIOD, GPIO_Pin_0, RESET);
}
```

Now if we want to do the same with GPIO_SetBits and GPIO_RestBits function, we will have to write the code like this:

```C++
while(1)
{
    Delay_Ms(500);
    GPIO_SetBits(GPIOD, GPIO_Pin_0);
    Delay_Ms(500);
    GPIO_ResetBits(GPIOD, GPIO_Pin_0);
}
```

Complete code will be like this:

```C++
 * @brief   Initializes GPIOD.0
 *
 * @return  none
 */
void GPIOConfig(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);
}

/*********************************************************************
 * @fn      main
 *
 * @brief   Main program.
 *
 * @return  none
 */
int main(void)
{

    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    SystemCoreClockUpdate(); // Configure MCU Clock HSI
    Delay_Init(); // 6mSec delay to allow clock to stabilize
    GPIOConfig(); // Configure the GPIO Pins

    while(1)
    {
        Delay_Ms(500);
        GPIO_SetBits(GPIOD, GPIO_Pin_0);
        Delay_Ms(500);
        GPIO_ResetBits(GPIOD, GPIO_Pin_0);
        
    }
}
```











