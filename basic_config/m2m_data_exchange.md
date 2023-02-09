# Data exchange

The GPDMA have capability to exchange data during transfer

## Destination byte exchange

Possible only if destination size is more than byte.
Exchange bytes in each half-word

![Byte exchange](./img/07.svg)

## Destination half-word exchange

Possible only is destination is word. 
Exchange half-words in word.

![Byte exchange & half word exchange](./img/09.svg)

## Source byte exchange

Possible only if source is word. 
The unaligned bytes are exchanges

![Source byte exchange](./img/13.svg)


## Exchange combination

The exchanges is possible tombine between each other only user must be careful in thier size limits.

![Combined exchange](./img/10.svg)

# Simple example to test data exchange

In cubeMX best results are by selecting the source and destination as Word. 

