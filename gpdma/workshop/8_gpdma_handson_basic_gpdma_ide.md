----!
Presentation
----!

# Select ADC1 periphery

Select `ADC1` in **Analog**

![select ADC](./img/CubeIDE_BasicADC1.apng)

# Enable 4 adc channels
Enable channels IN0, IN2, IN6 and IN13 in Single Ended mode

![select 4 channels](./img/CubeIDE_BasicADC2.apng)

# Configure the ADC 1/3

1. Set `Continuous conversion mode` to **Enable**

This option run ADC in loops. When ADC finish converting all its channesl it will start again from beginning.

2. Set `DMA continuos Request` to **Enable**

This option enables permanent request for DMA transfer

3. Set `Low power wait` to **Enable**

This option will stop ADC until the DATA are read form it. It is good to prevent overrun. And we are sure that we have still correct order of channels.

4. Set `Enable Regular Conversions` to **Enable**

![configure adc 1](./img/CubeIDE_BasicADC31.apng)

# Configure the ADC 2/3

1. Set `number of conversion` to **4**

This will set ADC to do 4 ADC conversion which we can set.

![configure adc 2](./img/CubeIDE_BasicADC32.apng)

# Configure the ADC 3/3

1. You can set ADC channel for each `Rank`

Each rank will have assing one ADC channel to convert. It is possible to select same chanel each time.

![configure adc 3](./img/CubeIDE_BasicADC33.apng)

# Select GPDMA1

1. Sececd `Go to GPDMA1` in **DMA Settings tab**
2. Enable channel with 2D addressing (6 or 7) in `Standard Request Mode`

![Enable GPDMA](./img/CubeIDE_BasicGPDMA1.apng)

# Configure GPDMA1 CH6 1/2

1. Select CH6
2. Set `Circular Mode` to **Enable**
3. Set `Request` to **ADC1**

![configure GPDMA ch6 1](./img/CubeIDE_BasicGPDMA2.apng)

# Configure GPDMA1 CH6 2/2

1. Set `Source Data Settings` - `Data Width` to **Half Word**
2. Set `Destination Data Settings` - `Destination Address Increment After Transfer` to **Enable**
3. Set `Destination Data Settings` - `Data Width` to **Half Word**

![configure GPDMA ch6 2](./img/CubeIDE_BasicGPDMA3.apng)

# Generate code

Click on **Generate Code** and switch to `main.c`

# Create buffer where to store ADC values

We create array `data` of 16bit elements wuth size as `SIZE`which is 64 elements

Like this

```c
uint16_t data[64];
```

Put the array and size into `main.c` to section ` USER CODE BEGIN PV` like bellow

```c-nc
/* USER CODE BEGIN PV */
uint16_t data[64];
/* USER CODE END PV */
```

# Start ADC and DMA

To start ADC+DMA we can use HAL function `HAL_ADC_Start_DMA`
We use it like this:

```c
  HAL_ADC_Start_DMA(&hadc1, data, 64);
```

we put it into section `USER CODE BEGIN 2` like bellow

```c-nc
  /* USER CODE BEGIN 2 */
  HAL_ADC_Start_DMA(&hadc1, data, 64);
  /* USER CODE END 2 */
```

`&hadc1` - is ADC handle which contains information about ADC1 and related DMA channel

`data` - buffer where to store data from ADC
`SIZE` - `data` buffer size 

# Compile code and run debug

1. Run debug

2. Opena **data** in `Live expresion` window

3. Run application by pressing `F8` or press Run 

![Run code](./img/CubeIDE_BasicFinish.apng)

# What we created

We have application of ADC with simple DMA transfer
<br>
![adc dma description](./img/adc_dma_desc.json)



