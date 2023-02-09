# Simple M2M transfer

![M2M](./img/03.svg)

GPDMA have simple M2M transfer like in normal STM32 DMA. 

## SAR - Source address

Source address where to read data

## DAR - destination address

Destination where to put data

## BDTR - block number of data

![BDTR](./img/04.svg)

Number of transfers done by GPDMA. 
When BDTR=0:
1. GPDMA stops if LLR=0
2. GPDMA load new LLI if LLR!=0

# Simple example

![Transfer](./img/05.svg)

We will use GPDMA to copy M2M buffer


In MX select GPDMA
Select any channel
Standart request mode

For data source and destination enable 
Address increment after transfer
Transfer Byte on both source and destination

Generate project

Create buffers for GPDMA

```c
uint8_t buffer_tx[8]={1,2,3,4,5,6,7,8};
uint8_t buffer_rx[16];
HAL_StatusTypeDef retVal;
```

Start GPDMA with:

```c
  retVal = HAL_DMA_Start(&handle_GPDMA1_Channel0, buffer_tx, buffer_rx, 4);
```
