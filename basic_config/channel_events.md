# Transfer events/interrupts

The channel event generation can be set to multiple options. This is helpfil to avoid creating HT/TC interrupts for specific conditions. 

## Block transfer event generation

The TC is generated at the end of block transfer the HC int the middle. 

![Block event](./img/28.svg)

<awarning>
No HC/TC is generated if LLR=0
</awarning>

## Repeated block event 

![Repeated block event](./img/29.svg)

<awarning>
No HC/TC is generated if LLR=0
</awarning>

## LLI load event generation

TC is generated when ne LLI is loaded to registers. 
HC is generated in middle of block/repeated block transfer.

![LLI transfer event](./img/30.svg)

<awarning>
No HC is generated if no data were transfered in LLI
</awarning>

## Last LLI event generation

The TC is generated when GPDMA dinish the LLI list and LLR=0.
HC is generated in middle of LLI

![Last LLI event](./img/31.svg)