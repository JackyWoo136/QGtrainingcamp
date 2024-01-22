# 51单片机-LED流水灯

## 设计思路

1. 能控制LED灯的亮灭；
2. 每次LED灯亮灭后有延时，才能有流水灯的效果
3. 延时时长可设置



## 硬件设置

在`P2`寄存器上连接八个LED灯，且每个LED灯都配有一个限流电阻，最终并联在一个电源上。



## 代码实现

功能一：定义一个`Delay`函数，使程序延时`xms`毫秒

```C
void Delay(unsigned int xms){
	unsigned char i, j;
	while (xms--){
		i = 2;
		j = 239;
		do{
			while(--j);
		}while(--i);
	}	
}
```

功能二：让`P2`寄存器上的接口依次接入一个低电平，达到流水灯的效果，可运用for循环

```C
unsigned char i;
for(i=0;i<8;i++)
P2 = ~(0x01 << i);//当i=1时，0x01 << 1 -> 0000 0001 << 1 -> 0000 0010，取反得1111 1101，即仅第二个接口为低电平 
```



***合并功能：***

```C
#include "REG51.h"

void Delay(unsigned int xms){
	unsigned char i, j;
	while (xms--){
		i = 2;
		j = 239;
		do{
			while(--j);
		}while(--i);
	}	
}
void main(void){
	unsigned char i;
	while(1){
		for(i=0;i<8;i++){
			P2 = ~(0x01 << i);
			Delay(100);//延时100毫秒
		}
	}
}
```

