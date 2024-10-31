
# Stage 1: Hardware Initialization

This is the phase that works between pressing the power button and the screen actually lighting up.
It wakes the processor up which in turn wakes up all the peripheral devices on the computers motherboard, including the gpu which controls the monitor. 
The hardware initialization code introduces the microcontroller to ints peripherals and then makes the microprocessor wake up the peripherals so that the next stage of the bootloader can begin.

# Stage 2: Bootloader mode or application mode- Decision making


Now once the hardware is initialized, there is a decision that needs to be made, which is whether the system should go to:
- Default or application mode - Starts and allows the user to access and use the microprocessor with the code flashed on it
- Special or bootloader mode - Go into the bootloader settings and play around with the boot setting.
On PCs this decision is made after the motherboard manufacturer's logo is displayed
On embedded applications this can be done using reading and mapping the GPIO pins.

Other peripherals can also be used here depending upon the application and capabilities of the product. For example, manufacturers of microcontrollers usually provide bootloader drivers that can be customized to make the system listen to Serial ports, USB and Ethernet for entering the bootloader mode as the state of GPIOs are susceptible to noise voltages.

The user can perform tasks like

- setting up some default options for the application software to use
- to reflash the application software during the product development phase.  This happens every time we load software into the microcontroller.
- Updating the software, usually to add features or clear bugs after the product release phase.

At this point the job of a bootloader is complete and the application mode code can start executing.

# Stage 3: Startup Code

After the hardware initialization phase the system is still not ready to execute the application program.

The start up code has the function to prepare the execution environment for the application written in higher level languages(up until here the code is written in assembly)

A generic startup code has the following functions:
- Allocates space and copies all the global variables in the code into RAM
- Stack and Stack pointer is initialized
- Heap is initialized
- Call the main function in your program

# Stage 4: OS Starting Process

Once the runtime is setup, we need to initialize and set up the operating system. This step is optional in embedded systems because most are designed without a operating system

The time where the windows or mac logo flashes- in the brief time the os starting code is being executed.
Our drivers, network interface cards and all the other peripherals are initialized.
In embedded operating systems like RTOS, embedded linux do all these same tasks but the peripherals are different.

On embedded systems without an Operating System, this further initialization takes place inside the main function, to start up all the necessary peripherals to get a fully functioning system.
