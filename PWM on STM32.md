
We will be using the PA5 led which is on the stm board at the pin PA5

Configure the PA5 pin to TIM2_CH1

Go to the timers section and enable the channel to be set with PWN Generation Channel 1

## Configuring the PWM 

PWM = Input frequency to the timer unit / (1+prescaler)* counter_period

 = 16000000 / (1+0)* 256 = 62.5kHz is the pwm frequency 

We set the clock counter to 255 
enable the HAL_TIM_PWM_START for the channel 1 of the tim2

run a for loop, and set the value of the CCR--> capture compare register which helps us to change the duty cycle of the clock.

A increment for loop increase the ccr value, then another decrements it.
```
HAL_TIM_PWM_Start(&htim2, TIM_CHANNEL_1);

/* USER CODE END 2 */

  

/* Infinite loop */

/* USER CODE BEGIN WHILE */

while (1)

{

/* USER CODE END WHILE */

for(int i = 0; i< 255;i++){

__HAL_TIM_SET_COMPARE(&htim2,TIM_CHANNEL_1,i); // this third param i is basically the ccr register

HAL_Delay(5);

}

for(int i = 255; i>0;i--){

__HAL_TIM_SET_COMPARE(&htim2,TIM_CHANNEL_1,i); // this third param i is basically the ccr register

HAL_Delay(5);

}

  

/* USER CODE BEGIN 3 */

}
```
