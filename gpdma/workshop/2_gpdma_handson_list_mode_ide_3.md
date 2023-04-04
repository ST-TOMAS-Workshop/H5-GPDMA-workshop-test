----!
Presentation
----!

# Connect queue and GPDMA channel

To connect Queue and GPDMA we will use `HAL_DMAEx_List_LinkQ`

Add

```c
HAL_DMAEx_List_LinkQ(&handle_GPDMA1_Channel6, &YourQueueName);
```

to section `/* USER CODE BEGIN 2 */` like

```c-nc
  /* USER CODE BEGIN 2 */
  MX_YourQueueName_Config();
  
  HAL_DMAEx_List_LinkQ(&handle_GPDMA1_Channel6, &YourQueueName);
  /* USER CODE END 2 */
```

First parameter `handle_GPDMA1_Channel6` is GPDMA handle
Second parameter `YourQueueName` is our Queue

# Start GDMA and ADC

Start DMA by using `HAL_DMAEx_List_Start`<br>
ADC is started by using `HAL_ADC_Start`, which is preceded by the DMA transfer enable.

Add 

```c
  HAL_DMAEx_List_Start(&handle_GPDMA1_Channel6);
  ADC1->CFGR |= ADC_CFGR_DMAEN;
  HAL_ADC_Start(&hadc1);
```

To section `/* USER CODE BEGIN 2 */` like 

```c-nc
  /* USER CODE BEGIN 2 */
  MX_YourQueueName_Config();

  HAL_DMAEx_List_LinkQ(&handle_GPDMA1_Channel6, &YourQueueName);

  HAL_DMAEx_List_Start(&handle_GPDMA1_Channel6);
  ADC1->CFGR |= ADC_CFGR_DMAEN;
  HAL_ADC_Start(&hadc1);
  /* USER CODE END 2 */
```
<br>
<br>
## Compile code and run debug

# In debug open Live Expression

Live expression showing changes in watched variables

![Open Live expression](./img/CubeIDE_Live1.apng)

# Add variable to live watch

![Add to live watch](./img/CubeIDE_Live2.apng)

# Run code and check changing content

Run code by pressing `F8` or press Run 

![Run code](./img/CubeIDE_Live3.apng)

# What we created

We have application of ADC with DMA transfer, based directly on linked list, where we can add more elements
<br>
![adc dma description](./img/adc_dma_desc.json)

# Add link to GPDMA

We lined node to our GPDMA channel with `HAL_DMAEx_List_LinkQ`

![link queue](./img/link_queue.json)
