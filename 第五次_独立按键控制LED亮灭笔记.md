# 51单片机-独立按键控制LED

## 设计思路

1. 当按键按下时，LED状态取反；
2. 需要一个延时装置来消除按键震动（5 ms~20 ms）；

## 硬件设置

1. 在`P1.0`口接上按键以及接地；
2. 在`P2.0`口接上LED、限流电阻和电源；

## 代码实现

1. 延时装置（xms表示延时x毫秒）：

	```C
	void Delay(unsigned int xms){
		unsigned char i, j;
		while(xms--){
			i = 2;
			j = 239;
			do{
				while (--j);
			}while(--i);
		}
	}
	```

2. 当按键按下时，LED状态取反：

	```C
	if(P1_0==0){
				Delay(20);
				while(P1_0==0);
				Delay(20);
				
				P2_0 = ~P2_0;
			}
	```

***完整代码***

```c
#include "REG51.h"
sbit P1_0 = 0x90;
sbit P2_0 = 0xA0;

void Delay(unsigned int xms){
	unsigned char i, j;
	while(xms--){
		i = 2;
		j = 239;
		do{
			while (--j);
		}while(--i);
	}
}
void main(void){
	while(1){
		if(P1_0==0){
			Delay(20);
			while(P1_0==0);
			Delay(20);
			
			P2_0 = ~P2_0;
		}
	}
}
```

