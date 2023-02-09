----!
Presentation
----!

# Add LPTIM

Enable LPTIM1
Set LPTIM to prescaller 128
And Period to 65536 
To have tick each 2s if MSIK is 4MHz
Enable Channel1

Set LPTIM source to MSIK

Start LPTIM
```c
  __HAL_LPTIM_ENABLE(&hlptim1);
  __HAL_LPTIM_CAPTURE_COMPARE_ENABLE(&hlptim1,LPTIM_CHANNEL_1);
  __HAL_LPTIM_START_CONTINUOUS(&hlptim1);
```

<awarning>
To be able run LPTIM in STOP you must add

```c
  __HAL_RCC_LPTIM1_CLKAM_ENABLE();
```
</awarning>


# Add ADC4

Select ADC4 in MX. 
Configure 2 channels
Vrefint 
and tem sensor
Set scan mode
Set trigger from LPTIM1

Allow VDDA to power ADC

```c
  __HAL_RCC_PWR_CLK_ENABLE();
  HAL_PWREx_EnableVddA();
```

Allow ADC4 to run in stop

```c
  __HAL_RCC_ADC4_CLKAM_ENABLE();
```

# ADC buffer and start ADC

Add section to linker file to muve ADC buffer to SRAM4

Add memory

```c
  RAM4	(xrw)	: ORIGIN = 0x28000000,	LENGTH = 16K
```

Add section

```c
  .sram4 :
  {
    _ssram4 = .;        /* create a global symbol at data start */
    *(.sram4)           /* .data sections */
    _esram4 = .;        /* define a global symbol at data end */
  } >RAM4
```

Adc buffer and set it to sram4

```c
uint16_t buffer_adc[64] __attribute__((section(".sram4")));
```

SRAM4 in stop

```c
__HAL_RCC_SRAM4_CLKAM_ENABLE();
```

Start ADC4

```c
HAL_ADC_Start_DMA(&hadc4, buffer_adc, 6);
```

# Set LPDMA for ADC4

Select yuour channel
Request from ADC4
Set circular mode.
Set source/dest half-word
Increment destination
Allow event generation at each LLI end

Allow LPDMA in STOP
```c
__HAL_RCC_LPDMA1_CLKAM_ENABLE();
```

Put LPDMA node to sram4

```c
DMA_NodeTypeDef Node_LPDMA1_Channel0  __attribute__((section(".sram4")));
```

# Go to LP

Disable HT to have on TC from LPDMA
```c
__HAL_DMA_DISABLE_IT(&handle_LPDMA1_Channel0,DMA_IT_HT);
```

Disable HAL delay

```c
HAL_SuspendTick();
```

Enter stop 

```c
HAL_PWREx_EnterSTOP1Mode(PWR_STOPENTRY_WFI);
```

Wakeup and resume HAL

```c
HAL_ResumeTick();
```

Add GPIO toggle to see device is running

```c
HAL_Delay(200);
HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
```

# Add LPUART

Enable LPUART
Set baudrate

For uart to work we must set 

```c
 __HAL_RCC_LPUART1_CLKAM_ENABLE();
```

Transmit data

```c
HAL_UART_Transmit_DMA(&hlpuart1, buffer_adc, 12); // 6 samples *2
```

# Add second LPDMA

Enable LPDMA channel
Set circular mode
Peripheral to memory
Set LPUART_TX request
Source increment. 
Source/dest byte. 

Event at the end of LLI.


Pul LPDMA note to sram4

```c
DMA_NodeTypeDef Node_LPDMA1_Channel1  __attribute__((section(".sram4")));
```

# Create list

Node a

Source address

```c
 &(ADC4->DR)
```

Dest address 

```c
buffer_adc
```

Size 

```c
12
```

Node b

```c
buffer_adc
```

Dest address 

```c
&(LPUART1->TDR)
```

Size 

```c
12

# Put two lists together

## Include the list

```c
#include "linked_list.h"
```

```c
extern DMA_QListTypeDef YourQueueName;
```

```c
MX_YourQueueName_Config();
HAL_DMAEx_List_LinkQ(&handle_LPDMA1_Channel0, &YourQueueName);
HAL_DMAEx_List_Start(&handle_LPDMA1_Channel0);
```

Disable LPDMA interrupt for HT

```c
  __HAL_DMA_DISABLE_IT(&handle_LPDMA1_Channel0,DMA_IT_HT);
//__HAL_DMA_DISABLE_IT(&handle_LPDMA1_Channel0,DMA_IT_TC);
```

copy extern to linked list

```c
extern uint16_t buffer_adc[64];
```

# Start ADC

```c
hadc->Instance->CFGR1 |= ADC4_CFGR1_DMAEN;
HAL_ADC_Start(&hadc4, buffer_adc, 6);
```


# Start LPUART

```c
  hlpuart1.Instance->CR3|= USART_CR3_DMAT;
```

