# LLI & HAL

## GPDMA config

GPDMA config is done by ``HAL_DMAEx_List_Init``

![GPDMA LLI config & ](./img/38.svg)

As parameter used GPDMA handle

## Node creation

Node is created with ``HAL_DMAEx_List_BuildNode`` where the function will translate the config structure into GPDMA LLI 

## Add note to queue

The created node is added to queue by ``HAL_DMAEx_List_InsertNode_Tail``

![queue add node](./img/39.svg)

## Circular mode

``HAL_DMAEx_List_SetCircularMode`` will create circular mode to first node in queue

![circular mode](./img/42.svg)

If user will use ``HAL_DMAEx_List_SetCircularModeConfig`` he can specify first circular node in loop

![first circular node](./img/41.svg)

## Add queueu and GPDMA together

Co tombine GPDMA and queue ``HAL_DMAEx_List_LinkQ`` is used. 

![first circular node](./img/43.svg)

GPDMA linked list mode is started with ``HAL_DMAEx_List_Start``
