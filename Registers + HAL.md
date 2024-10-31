
# How does a microcontroller work

## Memory

 RAM -  Random access memory. This is our generic dumping ground. All the variables we create we store it in the memory(RAM)

Blue Pill - 64kB of memory

Special Locations in memory are called registers: 

#point PORTA is the port that controls all of the A pins. GPIOA ODR - Output Data registers

### Registers

They control how everything in the microcontroller works
GPIO, Clock speed, Timers, Serial, SPI,... everything is controlled by registers

Storing specific numbers in these registers controls how our microcontroller works.


## HAL- Hardware Abstraction Layer


```
digitalWrite(3, HIGH)
```

The above is how we set the pin3 high in Arduino

```
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_3, GPIO_PIN_SET)
```

This is how we do it in stm


Why so simple in arduino- because arduino compromises performance for code readability.
So internally the Pin 3 is port-D and pin - 3 . So the made up pin 3 is linked to this port and pin.

GPIO_PIN_SET , HIGH --> 1
GPIO_PIN_RESET, LOW --> 0

## STM's workflow

STM32 Cube MX -> Configure and Generate Code

STM32 Cube IDE -> Compile and Debug IDEs

STM32 Cube Programmer -> Monitor and Program Utilities






