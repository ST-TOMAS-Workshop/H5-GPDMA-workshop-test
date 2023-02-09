
# GPDMA

GPDMA have 16 channels

Channels 0 - 11 have normal addresing mode with increment and burs

Channels 12 - 15 have possiblity to support block transfer with 2D addressing. 

![GPDMA](./img/01.svg)

# GPDMA Flags/Interrupts

![Interrupts & Flags](./img/02.svg)

Each GPDMA channels have possible interrupts

Basic transfer intertupts:

* Half transfer complete 
* Transfer complete

Informations flags

* channel suspend interrupt
* Idle flag

Error interrupts:

* Trigger overrun
* User setting error
* Update link transfer error
* Data transfer error

Important is to check all intertupt flags in case that GPDMA is not working as expected. 

<ainfo>
Idle flag can help to check if GPM dma is working or not. GPDMA is in indle only when disable or get error
</ainfo>