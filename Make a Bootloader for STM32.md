
Bootloader is a program that runs another program. It provides other functionalities such as creating backups and updating software without any additional hardware

**What we want to achieve: When the power is turned on the microcontroller begins to execute the code at the beginning of the memory. This is the bootloader. Under normal conditions the bootloader loads and runs the user's application**

In STM32(as in other microcontrollers) the bootloader and the user code both are stored in the flash memory. Bootloader hence has the task to make the jump to the next location in the flash memory to run the user's code.

Under other circumstances however, the bootloader should:
- Download the user's application through the appropriate interface (UART,SPI,SD card, Ethernet, etc.)
- Upload it in the flash memory in the appropriate place, replacing the previous version
- Optionally verify if the uploaded program is correctly written
- Run the application, which means jumping to its beginning

**Its important to make the amount of memory occupied by the bootloader as small as possible**

# Some details

A microcontroller always begins to execute code at the beginning of the memory at power on. But the actual main function might be located a little further
To solve this problem at the beginning of the memory there a interrupt vectors. These are pointers for functions(memory location where the function is located) which are called when a interrupt occurs

Example: 
Lets say the interrupt vector of SysTick timer is stored at the address of 0x003C in the interrupt array and the four byte value of 0x00010020 is written there. If this interrupt occurs the microcontroller calls the function located at 0x00010020 memory address. Basically it will make a jump to the stored memory address

In the STM32 microcontroller at the address 0x0000 there is a value that should be entered into the Stack Pointer register. At the 0x0004 address there is the first interrupt vector- Reset vector. This is the address the microcontroller jumps to after Reset.

VTOR register is also important as it allows us to reallocate the interrupt vector array to a different starting address. 

If in the VTOR register we enter the value 0x00010000 then when the SysTick interrupt occurs, instead of 0x003C the microcontroller will look for the address- 0x0001003C. 



# Let's create something

We will be creating two projects- the first step is to blink 2 different led's at different frequencies.
Now if we upload the second program the first program is overridden. To change that we need to update the bootloader.
We firstly change the linker script in the _STM32F207VCTX_FLASH.ld_ file and allocate the first 64kb to the bootloader program
We want to allocate the rest of the memory for the user's application: 256 - 64 = 192kb of memory will go towards the user's application. We will also move it away from the bootloader. So we shift the memory location by 64kb. So we update the memory start address.

These modifications- linker should place our program in memory properly. After this for everything to work properly we need to indicate the offset in the interrupt array in: _/Core/Src/system_stm32f2xx.c_ file 
64k --> 0x10000

# Jump to user's application


Now we have the bootloader and user's application properly placed we can now jump to coding the user's application

1. Deinitialize all peripherals used in the bootloader.
2. Reset the SysTick timer.
3. Set the interrupt vector array offset (SCB→VTOR).
4. Set the stack pointer (value from the interrupt array from the 0x0000 address [ relative to the beginning of the user’s application ]).
5. Jump to the beginning (entry point) of the user’s application, i.e. to the address indicated by the Reset vector. As I mentioned earlier, it is located at the address of 0x0004 relative to the beginning of the program.

We write the code for this:
```c
The code that performs the above actions looks like this:

#define APP_ADDRESS (uint32_t)0x08010000

typedef void (*pFunction)(void);
void JumpToAddress(uint32_t addr) {
    uint32_t JumpAddress = *(uint32_t *) (addr + 4);
    pFunction Jump = (pFunction) JumpAddress;

    HAL_RCC_DeInit();
    HAL_DeInit();
    SysTick->CTRL = 0;
    SysTick->LOAD = 0;
    SysTick->VAL = 0;

    SCB->VTOR = addr;
    __set_MSP(*(uint32_t *) addr);

    Jump();
}

void JumpToApplication() {
    JumpToAddress(APP_ADDRESS);
}
```

We will modify the bootloader program to blink the led three times before making the jump to user's application

```c
while (1) {
    for (int i = 0; i < 6; i++) {
        HAL_GPIO_TogglePin(GPIOE, GPIO_PIN_7);
        HAL_Delay(500);
    }

    JumpToApplication();
}
```
