
# Boot Modes

When you press the reset button the processor starts running from the 0x00000000 address.
We can change this by using these boot modes. Every microcontroller has such boot pins using which we can tell the microcontroller to boot from a specific user defined address

For the STM32F76xx there is only one boot-pin BOOT0

If the boot0 is grounded then the it will start from 0x00200000 address. If BOOT0 is high during power up it will start from the 0x00100000 address where the system bootloader sits.

To run our custom bootloader we will place our program in the 0x08000000 address.
0x08000000 and 0x00200000 both point to the same memory location. The bus used to access the 0x002 address is **ICTM** bus where we only have read and execute permission as a user. 0x08000000 memory is accessed by the **AXI** bus which gives us read, write and execute permission.

Memory aliasing concept is at play here. We said that the processor runs from the 0x00000000 address. If the BOOT0 pin is grounded then the 0x00200000 acts as  0x00000000. Else 0x00100000 acts as 0x00000000.

# What is System Bootloader

The system bootloader in STM32 is the bootloader that has been provided by the chip manufacturers. We cannot overwrite as it has loaded into the ROM memory. We can also call this a ROM bootloader. Using this bootloader, we can update the firmware or application from the bootloader.

But we cannot do many operations like updating the application wirelessly like OTA. So, writing our own bootloader in STM32 will help us to implement whatever we like.

# Bootloader Design

## Organising the memory

The tutorial has kept 2 versions of backup of the user application. Slot 1 for current version and Slot2 for previous version and Slot3 for the second backup version. Why?- No Idea, maybe because STM32F76xx has 2Mb of flash memory.

I cannot copy this memory allocation because STM32G071RB has only 128Kb of flash memory.

1. The bootloader will be placed into the sector 0, 1 (**`0x08000000`** to **`0x0800FFFF`**) (**64KB**).
2. The actual application will be placed into the sector 5,6 (**`0x08040000`** to **`0x080BFFFF`**) (**512KB**).
3. Backup application will be placed into the sector 7,8 (**`0x080C0000`** to **`0x0813FFFF`**) (**512KB**).
4. Backup application 2 will be placed into the sector 9,10 (**`0x08140000`** to **`0x081BFFFF`**) (**512KB**).
5. Configurations will be stored in sector 3 (**`0x08020000`** to **`0x0803FFFF`**) (**128KB**).

After finalising the memory area to keep the bootloader, application and the many backups- we have to plan the operations of the bootloader and application.


## Bootloader and Application Operations

![[Pasted image 20241031011446.png]]




