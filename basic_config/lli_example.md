# LLI GPIO toggle example 

## Two DMA

![LLI circular 2DMA](./img/36.svg)

MX 
Use Ch0 & Ch1
Set single transfer word to word.
Source increment

Trigger on Exti 
Trigger single transfer

Circular mode

Ch0 Source

```c
buffer_a
```

Ch0 Destination 

```c
&(GPIOB->BSRR)
```

Ch0 Data size

```c
8
```

Ch1 Source

```c
buffer_b
```

Ch1 Destination 

```c
&(GPIOG->BSRR)
```

Ch1 Data size

```c
8
```



Start GPDMA

```c 
HAL_DMAEx_List_Start(&handle_GPDMA1_Channel0);
HAL_DMAEx_List_Start(&handle_GPDMA1_Channel1);
```

Trigger the EXTIM over PC13

```c
	  HAL_Delay(10);
	  pin_val =  HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_13);
	  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, pin_val);
```

Values for GPIO

```c
uint32_t pin_val;
uint32_t buffer_a[2]={0x00000080,0x00800000};
uint32_t buffer_b[2]={0x00000004,0x00040000};
```

## One GPDMA


![One GPDMA](./img/37.svg)

In MX use Ch2
Use Ch2 Linked list mode
Set Ch2 to circular

In LinkedList

Set queue circular
First node 

``
a
``

## add node ``a``

Source:
Word

Dest:
Word

Trigger:
Rising
Exti0
Block

Runtime:

Source address

```c
&buffer_a[0]
```

Destination address

```c
&(GPIOB->BSRR)
```

Size ``4``

## Add node ``b``

Source:
Word

Dest:
Word

Trigger:
No trigger

Runtime:

Source address

```c
&buffer_b[0]
```

Destination address

```c
&(GPIOG->BSRR)
```

Size ``4``

## Add node ``c``

Source:
Word

Dest:
Word

Trigger:
Rising
Exti0
Block

Runtime:

Source address

```c
&buffer_a[1]
```

Destination address

```c
&(GPIOB->BSRR)
```

Size ``4``

## Add node ``d``

Source:
Word

Dest:
Word

Trigger:
No trigger

Runtime:

Source address

```c
&buffer_b[1]
```

Destination address

```c
&(GPIOG->BSRR)
```


Generate code

in ``main.c``

Include 

```c
#include "linked_list.h"
```

Add extern variable

```c
extern DMA_QListTypeDef YourQueueName;
```

Call queue init
Add queue to GPDMA channel
Start GPDMA

```c
  MX_YourQueueName_Config();
  HAL_DMAEx_List_LinkQ(&handle_GPDMA1_Channel2, &YourQueueName);
  HAL_DMAEx_List_Start(&handle_GPDMA1_Channel2);
```

In ``linked_list.c``
Add buffer variables

```c
extern uint32_t buffer_a[];
extern uint32_t buffer_b[];
```