
Bootloader is a special operating system software that loads into the memory of the computer after start-up. This is a software like application. Bootloader is short for Bootstrap loader.

# How does a Bootloader work

At the time of booting if you press the shift or F12 key- you see a screen with some options. That is the bootloader in action. It will do some operations based on the need. Once done with its work, the bootloader transfers control to the OS(user application in the case of embedded systems).

# What is Bootloader in Embedded Systems

- It is the first piece of code which runs when the microcontroller is powered on
- If you don't have a bootloader then the application code will directly start
- If you have the bootloader then the bootloader will run do its operations 
- Then the control will pass to the user's applicaiton


# Need for a Bootloader

## Firmware Update

As a hobbyist it is alright not use a bootloader. Stuff is simpler- less code to write and manage.
When you plan to sell your application to customers, you might need a bootloader. If you want to update the firmware on the device, but do not have a bootloader, you will have to go and manually connect the microcontroller and update the user firmware. This is not possible hence a bootloader is needed.

## Security
When we have a product which has to be secured, then what will you do when someone wants to override the firmware and load their custom firmware to hack your product? In this case we can use the bootloader to check if a firmware is safe and authorized to be loaded