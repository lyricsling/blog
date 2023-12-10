---
title: "Gpio管理"
date: 2023-07-15T11:18:57+08:00
type: "post"
---

在早期， STM32 官方提供的程序库是 Standard Peripheral Library。

HAL(Hardware Abstraction Layer) 是对早期库的一个提升，更建议使用这个库。因为 st 公司会长期维护这个程序库。

新的HAL大大简化了STM32子系列之间的代码移植。

## 没有 HAL 库时，映射外设的方法

{{< figure src="/img/gpio_management/bus_architecture.png" title="最简单的总线架构" >}}

DAM

> 直接记忆体存取（Direct Memory Access，DMA）是计算机科学中的一种内存访问技术。它允许某些电脑内部的硬体子系统（电脑外设），可以独立地直接读写系统记忆体，而不需中央处理器（CPU）介入处理 。在同等程度的处理器负担下，DMA是一种快速的数据传送方式。

> DMA是所有现代电脑的重要特色，它允许不同速度的硬体装置来沟通，而不需要依于中央处理器的大量中断负载。否则，中央处理器需要从来源把每一片段的资料复制到暂存器，然后把它们再次写回到新的地方。在这个时间中，中央处理器对于其他的工作来说就无法使用。

{{< figure src="/img/gpio_management/bus_architecture.png" title="最简单的总线架构" >}}


根据手册，原始代码如下
``` cpp
int main(void) {
	volatile uint32_t *GPIOA_MODER = 0x0, *GPIOA_ODR = 0x0;
	
	// Address of the GPIOA->MODER register
	GPIOA_MODER = (uint32_t*)0x48000000;
	// Address of the GPIOA->ODR register
	GPIOA_ODR = (uint32_t*)(0x48000000 + 0x14);	
	
	//This ensure that the peripheral is enabled and connected to the AHB1 bus
	__HAL_RCC_GPIOA_CLK_ENABLE();
	
	// Sets MODER[11:10] = 0x01
	*GPIOA_MODER = *GPIOA_MODER | 0x400;
	// Sets ODR[5] = 0x1, that is pulls PA5 high
	*GPIOA_ODR = *GPIOA_ODR | 0x20;
	while(1);
}
```
不同的系列，GPIO映射点位不一样。导致了不同系列之间的代码不兼容

## 使用 HAL 库，映射外设
HAL 对上述操作进行了抽象

``` cpp
typedef struct {
	volidate uint32_t MODER;
	volidate uint32_t OTYPER;
	volidate uint32_t OSPEEDR;
	volidate uint32_t PUPDR;
	volidate uint32_t IDR;
	volidate uint32_t ODR;
	volidate uint32_t BSRR;
	volidate uint32_t LCKR;
	volidate uint32_t AFR[2];
	volidate uint32_t BRR;
} GPIO_TypeDef;
```