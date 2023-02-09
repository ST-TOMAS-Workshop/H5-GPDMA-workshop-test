# Block transfer

Each BDTR size is called block for GPDMA. 
Each block can have multiple bursts. The number of bursts mus fit to BDTR size. 

![Block](./img/19.svg)

# Block trigger

The block transfer can be conditioned by trigger

![Block trigger](./img/25.svg)

## Block address offset

The channels 12-15 have possiblity to add offset between each burst inside block. The offset is +-8192.

![Block offset](./img/20.svg)

## Repeated block

The channels 12-15 have possiblity to use repeated block feature. Which means the block transfer is repeated multiple times (2048)

![repeated block](./img/21.svg)

# Repeated block trigger

The repeated block trigger can be conditoned by trigger

![repeated block trigger](./img/26.svg)

# Repeated block address offset

Also repeadet block can have additional address offset after the block is executed. 
The offset can be from +-65536

![repeated block address offset](./img/22.svg)

# Simple example

Decode JPEG like strucuture using repeated block. 

![gpdma 2d example](./img/23.svg)

```c
uint8_t buffer_tx[12]={1,2,7,8,3,4,9,10,5,6,11,12};
uint8_t buffer_rx[16];
HAL_StatusTypeDef retVal;
```