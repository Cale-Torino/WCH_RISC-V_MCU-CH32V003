The CH32V003 MCU provides a read only 64-bit unique ID which can be used for various use cases in the applications.

If you don’t know the importance of Unique ID. MCU unique IDs can be used in a variety of real-world applications, including:

- Security: MCU unique IDs can be used to create secure systems that are resistant to cloning and counterfeiting. For example, a unique ID could be used to identify a legitimate user or device, or to encrypt data stored on the MCU.
- Asset tracking: MCU unique IDs can be used to track assets throughout their lifecycle. For example, a unique ID could be used to track a product through the manufacturing process, or to track the location of a piece of equipment in the field.
- Licensing: MCU unique IDs can be used to implement software licensing schemes. For example, a software application could require the user to enter a unique ID in order to run the software.
- Debugging: MCU unique IDs can be used to help debug embedded systems. For example, a unique ID could be used to identify a specific MCU on a board, or to track the progress of a software program through the MCU’s memory.

Here are some specific examples of real-world applications that use MCU unique IDs:

- Smart locks: Smart locks often use MCU unique IDs to authenticate users and to prevent unauthorized access.
- Wearable devices: Wearable devices such as smartwatches and fitness trackers often use MCU unique IDs to identify individual devices and to store user data.
- Industrial control systems: Industrial control systems often use MCU unique IDs to identify and track devices, and to prevent unauthorized access to critical systems.
- Medical devices: Medical devices such as pacemakers and insulin pumps often use MCU unique IDs to ensure patient safety and to prevent counterfeit devices from being used.
In general, MCU unique IDs can be used to improve the security, reliability, and traceability of embedded systems in a wide range of applications.

# Reading Unique ID of CH32V003

Reading unique ID is very simple, it is as good as reading any register.

Other than Unique ID we have a register for Chip Identification as well to detect which MCU.

Here are the different function which could be used to read all of them

```C++
uint16_t GetMCUFlashSize( void )
{
    return( *( uint16_t * )0x1FFFF7E0 );
}

uint32_t GetMCUUID1( void )
{
    return( *( uint32_t * )0x1FFFF7E8 );
}
uint32_t GetMCUUID2( void )
{
    return( *( uint32_t * )0x1FFFF7EC );
}

uint32_t GetMCUUID3( void )
{
    return( *( uint32_t * )0x1FFFF7F0 );
}

#define IDCODE_DEVID_MASK    ((uint32_t)0x0000FFFF)

/*********************************************************************
 * @fn      DBGMCU_GetREVID
 *
 * @brief   Returns the device revision identifier.
 *
 * @return  Revision identifier.
 */
uint32_t GetREVID(void)
{
    return ((*(uint32_t *)0x1FFFF7C4) >> 16);
}

/*********************************************************************
 * @fn      DBGMCU_GetDEVID
 *
 * @brief   Returns the device identifier.
 *
 * @return  Device identifier.
 */
uint32_t GetDEVID(void)
{
    return ((*(uint32_t *)0x1FFFF7C4) & IDCODE_DEVID_MASK);
}

/*********************************************************************
 * @fn      DBGMCU_GetCHIPID
 *
 * @brief   Returns the CHIP identifier.
 *
 * @return Device identifier.
 *          ChipID List-
 *    CH32V003F4P6-0x003005x0
 *    CH32V003F4U6-0x003105x0
 *    CH32V003A4M6-0x003205x0
 *    J4M6-0x003305x0
 */
uint32_t GetCHIPID( void )
{
    return( *( uint32_t * )0x1FFFF7C4 );
}

```

This is how you can use them by calling the functions.

```C++
    uint16_t flashSize = 0;
    uint32_t uid1 = 0;
    uint32_t uid2 = 0;

    uint32_t chipID = 0;
    uint32_t RevID= 0;
    uint32_t DevID= 0;


    flashSize = GetMCUFlashSize();
    uid1 = GetMCUUID1();
    uid2 = GetMCUUID2();
    uid3 = GetMCUUID3();

    chipID = GetCHIPID();
    RevID= GetREVID();
    DevID= GetDEVID();
```


Links
- [ch32v003-programming-read-unique-id](https://pallavaggarwal.in/2023/09/25/ch32v003-programming-read-unique-id/)