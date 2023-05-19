----!
Presentation
----!

# GPIO retentions hands-on code 1/2

Please use this code


```c
/* USER CODE BEGIN Includes */
#include <stdio.h>
/* USER CODE END Includes */
```


```c
/* USER CODE BEGIN PD */
//#define IORETENABLE
/* USER CODE END PD */
```


```c
/* USER CODE BEGIN PFP */
int __io_putchar(int ch)
{
	HAL_UART_Transmit(&huart3, (uint8_t *)&ch, 1, 0xFFFF);
 	return ch;
}
/* USER CODE END PFP */
```

# GPIO retentions hands-on code 1/2


```c
/* USER CODE BEGIN 2 */
    printf("\n\r ****************** IO RETENTION EXAMPLE *****************\n\r");
    HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0, GPIO_PIN_SET);
      printf( "LD1 turned on             \r\n");
      if(READ_BIT(PWR->IORETR, PWR_IORETR_IORETEN)== PWR_IORETR_IORETEN)
      {
        printf( "IO retention bit set             \r\n");
        HAL_Delay(1000);
        HAL_PWREx_DisableStandbyIORetention();
        printf( "Disabling IO retention            \r\n");
        HAL_Delay(10);
      }
      printf( " *********************************************************\r\n");
      printf( " *********** Push user button to enter STANDBY ***********\r\n");
      printf( " *********************************************************\r\n");
      while(HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13)== GPIO_PIN_RESET)
      {
    	  HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_0);
    	  HAL_Delay(100);
      }
      printf( "MCU entering STANDBY: use reset button to wake-up  \r\n");
      HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0, GPIO_PIN_SET);
      #if defined(IORETENABLE)
        printf("Enabling IO retention  \r\n");
        HAL_PWREx_EnableStandbyIORetention();
      #endif

      printf( " \r\n");
      HAL_Delay(1000);
      HAL_PWR_EnterSTANDBYMode();
/* USER CODE END 2 */
```