# GPDMA Linked list

The GPDMA is using Linked list featured. 
This allow to use LLR register and tell the GPDMA channel where to load new configuration. 

![LLI](./img/32.svg)

## Linked list base address

The user must select List base address and locate there a LLI. The LBAR is 65k big.

## Linked list

The LLR specify where is next linked list by its offsed addres. 
Address is offser in LLR+LBAR. The LLI must be 32bit aligned. 

![LLI LLR](./img/33.svg)

The LLR also can specify which registers will be updated from LLI and which will stay the same. This can reduce the LLI size. 

The LLI for channel 12-15 have two additional registes TR3 and BR2 for 2D addressing. 


## Linked list modes

### Full linked list mode. 

The GPDMA execute the LLI and at the end load new LLI and continue till LLR=0.

### Step linked list mode

The GPDMA execute LLI. Load new LLI but disable the channel. 
User must enable channel to execute the LLI. This is repeated after each LLI reload 

![LLI step mode](./img/34.svg)


# LLI circular mode

The GPDMA dont have cirtular mode like on legacy STM32 DMA. 
Insted the LLI must be set into circle. In simple configuration the GPDMA will use only one LLI and LLI will opoint LLR to itself. 

![LLI circular mode](./img/35.svg)


