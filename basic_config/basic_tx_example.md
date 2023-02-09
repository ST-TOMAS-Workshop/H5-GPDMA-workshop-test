# Simple UART Tx with GPDMA

Example to show how to configure GPDMA to be used with UART Tx.

1. Create new project

Description is here [Link](./mx_create_project.md)

2. Select `UART` (on STM32U575-Nucleo `UART1` is connected to ST-Link VCP)
3. Set UART mode to `Asynchronous` (standart UART mode)
   1. We can keep default settings 115200baud, 8word length, 1 stop bit, no parity
   2. Pins on STM32U5 nucleo should be PA9 & PA10

![select uart1](./img/21_11_26_12_50_6.gif)

4. Select `GPDMA channel `(`Channel 11`) in `Standart Request Mode`
   
![select gpdma](./img/21_11_26_13_02_8.gif)

5. Go to selected channel (`CH11`) and select USART1_TX request

This will allow GPDMA to react on UART1_TX dma requests

![gpdma request select](./img/21_11_26_12.gif)

6. Set direction `Memory to periphery`

![select meory to periphery](./img/21_11_26_14.gif)

7. Set `source address increment ` to `enable` 

![select source address increment](./img/21_11_26_16.gif)

## What we have now

Now we configuer UART1 and GPDMA. 
When UART1 is able to send data he create request to GPDMA. 
GPDMA will handle the reqeust by taking byte from memory and sending it to UART. 
Each time GPDMA make transfer the source address(memory) is incremented

8. Set project name and generate code:

Description is here [Link](./mx_generate_code.md)

<ainfo>
Not neede if you use MX in CubeIDE
</ainfo>


9. Import project to CubeIDE [Link](./ide_import_project.md)

<ainfo>
Not neede if you use MX in CubeIDE
</ainfo>

10. Open your `main.c` (by default `ProjectName>Core>Src>main.c`)

11. We add `buffer_tx` into section `/* USER CODE BEGIN PV */`

```c
uint8_t buffer_tx[]="Test message\n";
```

Should look like 

```cc
/* USER CODE BEGIN PV */
uint8_t buffer_tx[]="Test message\n";
/* USER CODE END PV */
```

12. Start UART with GPDMA by `HAL_UART_Transmit_DMA` adding it into section `/* USER CODE BEGIN 2 */`

```c
HAL_UART_Transmit_DMA(&huart1, buffer_tx, strlen(buffer_tx));
```

should look like 

```cc
  /* USER CODE BEGIN 2 */
  HAL_UART_Transmit_DMA(&huart1, buffer_tx, strlen(buffer_tx));
  /* USER CODE END 2 */
```

## HAL_UART_Transmit_DMA arguments

`&huart1` is UART handle it contains uart state and also link to GPDMA channel

`buffer_tx` is buffer which will be transmited over UART

`strlen(buffer_tx)` number of bytes to be transmited


13. Run debug [Link](./ide_debug.md)

14. If you use terminal the result sohudl be visible there

There is used termite terminal. Is visible that when code is started the message is shown on terminal. 

![terminal test](./img/21_11_26_24.gif)

## Handle end of transmisstion in code

The function `HAL_UART_Transmit_DMA` is not blocking, the code leave this function but the UART is still transmiting data and GPDMA is still working. We can use polling to check the finish state or we can use the callbacks from uart. 

15. Got to MX and enable NVIC interrupt for UART 

![uart interrupt](./img/21_11_26_26.gif)

The GPDMA is enabled by default. 
But we need also the TC(transmit complete) interrupt from UART. Because when GPDMA get TC there are still data in UART. So the GPDMA TC interrupt will enable UART TC interrupt and here will get out callback. 

16. Regenerate code

17. Add callback function to `main.c` into section `/* USER CODE BEGIN 0 */`

 ```c
void HAL_UART_TxCpltCallback(UART_HandleTypeDef *huart){
	__NOP();//we can pun now breakpoint here
}
 ```

 ```cc
/* USER CODE BEGIN 0 */
void HAL_UART_TxCpltCallback(UART_HandleTypeDef *huart){
	__NOP();//we can pun now breakpoint here
}
/* USER CODE END 0 */
 ```

<ainfo>
Callback function for UART is in file `stm32u5xx_hal_uart.c or` `stm32u5xx_hal_uart_ex.c`. Best is to look into file header. Or search for functions started by `__weak`
</ainfo>

18. Compile code and start debug. 

You can test the code by adding the **breakpoint** on ` __NOP()`

![uart with interrupt](./img/21_11_26_28.gif)

We can not get assynchronous end of operation by using UART callback.

