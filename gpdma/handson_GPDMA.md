----!
Presentation
----!

# Analog - ADC1


- ADC_Settings
   - Clock prescaler -> Asynchronous clock mode divided by 8
   - chanels 0,2,6,13 -> Single-ended / Enabled<br>
   - Continuous Conversion Mode -> Enabled<br>
   - DMA Continuous Request -> Enabled<br>
   - Low power Auto wait -> Enabled
- Regular conversions
   - Number of Conversions -> 4<br>
   - Ranks for each channel
- DMA
   - Go to GPDMA1

# System Core - GPDMA1

- Channel 6 -> Linked-List mode<br>
- CH6<br>
   - Execution Mode -> Circular

# Timers - TIM15

- Counter settings<br>
   - Internal clock -> Enabled<br>
   - Prescaler -> 49999
   - Counter Period -> 4999
- Trigger output<br>
   - Trigger Event Selection -> Update event


```c
49999
```


```c
4999
```
# Conectivity - USART3 (PD8, PD9)
- Mode -> Asynchronous

# Utilities - LINKED LIST
- **Add List**

- YourQueueName
   - DMA Channel to run the liked list -> GPDMA1 2D capability
   - Linear or circular LinkedList setting -> Circular
   - First Loop Node -> YourNodeName


```c
YourNodeName
```
# YourNodeName
- Request Configuration
   - Request as a parameter -> GPDA1_REQUEST_ADC1
- Channel Configuration
   - Direction -> Peripheral to Memory
- Source Data Settings
   - Source Addres Increment After transfer -> Disabled
   - Data Width -> Half word
- Destination Data Settings
   - Destination Addres Increment After transfer -> Enabled
   - Data Width -> Half word  
- Trigger
   - Trigger configuration -> Trigger ... on the rising edge
   - Trigger Selection -> TIM15 TRGO  
- Runtime configuration
   - Source Address -> (uint32_t)&(ADC1->DR)
   - Destination address -> (uint32_t)data
   - Data size -> (64 * 2)


```c
(uint32_t)&(ADC1->DR)
```


```c
(uint32_t)data
```


```c
(64*2)
```
# YourNodeName2
- **Add node**

- General -> rename to YourNodeName2
- Request Configuration
   - Request as a parameter -> GPDA1_REQUEST_USART3_TX
- Channel Configuration
   - Direction -> Memory to Peripheral
- Source Data Settings
   - Source Addres Increment After transfer -> Enabled
   - Data Width -> Byte
- Destination Data Settings
   - Destination Addres Increment After transfer -> Disabled
   - Data Width -> Byte  
- Runtime configuration
   - Source Address -> (uint32_t)data
   - Destination address -> (uint32_t)&(USART3->TDR)
   - Data size -> (64 * 2)


```c
(uint32_t)data
```


```c
(uint32_t)&(USART3->TDR)
```


```c
(64*2)
```
# linked_list.c

```c
/* USER CODE BEGIN PM */
extern uint16_t data[];
/* USER CODE END PM */
```

# main.c

```c
/* USER CODE BEGIN Includes */
#include "linked_list.h"
/* USER CODE END Includes */
```

```c
/* USER CODE BEGIN PV */
uint16_t data[64];
extern DMA_QListTypeDef YourQueueName;
/* USER CODE END PV */
```

```c
  /* USER CODE BEGIN 2 */
  //Setup and initialization of Linked list DMA  
  MX_YourQueueName_Config();
  HAL_DMAEx_List_LinkQ(&handle_GPDMA1_Channel6, &YourQueueName);

  // USART3 will be controlled by DMA, therefore we enable it by direct register access
  ATOMIC_SET_BIT(huart3.Instance->CR3, USART_CR3_DMAT);
  __HAL_UART_ENABLE(&huart3);

  //Enable Linked list DMA  
  HAL_DMAEx_List_Start(&handle_GPDMA1_Channel6);
  // using of Linked List DMA requires special procedure (HAL library supports standard request mede currently)  
  ADC1->CFGR |= ADC_CFGR_DMAEN;
  HAL_ADC_Start(&hadc1);

  // Start TIM15 for triggering  
  HAL_TIM_Base_Start(&htim15);
  /* USER CODE END 2 */
```



 
