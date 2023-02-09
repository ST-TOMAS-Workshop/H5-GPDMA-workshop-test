# GPDMA memory to memory transfer simple example

## Example description

This example is to show how to create simple memory to memory copy with GPDMA. We will copy `buffer_a` to `buffer_b`.

## Step by stem creation

1. Create new project

Description is here [Link](./mx_create_project.md)

2. Open `GPDMA` configuration

![gpdma config](./img/04.png)

3. Select GPDMA channel in `Standard request mode` same as `normal mode` on legacy STM32

![gpdma channel select](./img/05.png)

4. In configuration select channel tab (`CH9`)

![gpdma channel configuration select](./img/06.png)

   
5. Configurate channel
   
For M2M transfer we need to increment source and destination after each transfer. By default GPDMA channel is set to SW. This means that GPDMA is not waiting to another periphery to request transfer.

Set:

**Destination address increment** to `Enabled`
  
**Source address increment** to `Enabled`

![gdma config increment](./img/07.png)

<ainfo>
This configuration allow us to copy all bytes from `buffer_a` to `buffer_b`
</ainfo>


6. Set project name and generate code:

Description is here [Link](./mx_generate_code.md)

<ainfo>
Not neede if you use MX in CubeIDE
</ainfo>


7. Import project to CubeIDE [Link](./ide_import_project.md)

<ainfo>
Not neede if you use MX in CubeIDE
</ainfo>


8. Open your `main.c` (by default `ProjectName>Core>Src>main.c`)

![main.c open](./img/17.png)

9. We add `buffer_a` and `buffer_b` into section `/* USER CODE BEGIN PV */`

```c
uint8_t buffer_a[4]={1,2,3,4};
uint8_t buffer_b[4];
```

Should look like 

```cc
/* USER CODE BEGIN PV */
uint8_t buffer_a[4]={1,2,3,4};
uint8_t buffer_b[4];
/* USER CODE END PV */
```

10. Start GPDMA by `HAL_DMA_Start` adding it into section `/* USER CODE BEGIN 2 */`

```c
HAL_DMA_Start(&handle_GPDMA1_Channel9, buffer_a, buffer_b, 4);
```

should look like 

```cc
/* USER CODE BEGIN 2 */
HAL_DMA_Start(&handle_GPDMA1_Channel9, buffer_a, buffer_b, 4);
/* USER CODE END 2 */
```

## HAL_DMA_Start arguments

`&handle_GPDMA1_Channel9` is GPDMA handle containing information about GPDMA state. In my case i have channel9. 

`buffer_a` - is source buffer from which we will copy data

`buffer_b` - destination buffer where to copy data

`4` - is number of bytes to copy max is 2^16(65k)


<awarning>
GPDMA is counting transfers in bytes even if your source and destination width is word
</awarning>


## Check if DMA finishes

10. To check DMA end we can use function `HAL_DMA_PollForTransfer`

```c
HAL_DMA_PollForTransfer(&handle_GPDMA1_Channel9, HAL_DMA_FULL_TRANSFER, 0x100);
```

Complete section will look like

```cc
  /* USER CODE BEGIN 2 */
  HAL_DMA_Start(&handle_GPDMA1_Channel9, buffer_a, buffer_b, 4);
  HAL_DMA_PollForTransfer(&handle_GPDMA1_Channel9, HAL_DMA_FULL_TRANSFER, 0x100);
  /* USER CODE END 2 */
```

The `HAL_DMA_PollForTransfer` arguments

`&handle_GPDMA1_Channel9` is handle with GPDMA state and configration

`HAL_DMA_FULL_TRANSFER` function is wating to complete GPDMA transfer event

`0x100` timeout where the function will retunr error


<ainfo>
For this example im not checking retunr functions to make it more simple
But in normal situation we would catch also return parameter
</ainfo>

11. Run debug [Link](./ide_debug.md)


12.  Check if `buffer_b` have same content as `buffer_a`

![check buffer copy](./img/21.png)

<ainfo>
The code can be runned by `F8`
Double click on left side of editor line `add/remove breakpoint`
Double click on variable allow to `add wariable to watch`(working only if code is **suspended**)
</ainfo>


## Conclusion

We configured GPDMA in MX to after each transfer copy increment soource and destination address. We used `HAL_DMA_Start` to start GPDMA operation and select buffers. We were able to check the GPDMA finish operation by using `HAL_DMA_PollForTransfer`