# 51单片机-通过定时器控制LED的闪烁和流水灯

## 定时器

> 是单片机内部的系统，每隔一段时间进行一次中断，中断后会跳到中断函数执行内部操作，使用前应先进行初始化

```c
void Timer0_Init(){
//TMOD = 0x01;          //定时器模式为0000 0001，可用下两行代替
	TMOD &= 0xF0;		//把TMOD的低四位清零，高四位保持不变
	TMOD |= 0x01;       //把TMOD的最低位置1 ，高四位保持不变
	TF0 = 0;            //中断溢出标志位
	TR0 = 1;            //定时器是否开启
	/*该计数器只能从0计到65535个数，每隔1us计数加1，总共定时65535us*/
	TH0 = 64535/256;    //64535的高八位，使计数器计1ms
	TL0 = 64535%256;    //64535的低八位，使计数器计1ms
	ET0 = 1;
	EA = 1;
	PT0 = 0;
}
```

## 中断函数

> 当计数器计完一个周期后，会跳到该函数

```c
void Timer0_Routine() interrupt 1{
	static unsigned int T0Count;
	TH0 = 64535/256;
	TL0 = 64535%256;
	T0Count++;
	if(T0Count>=100){  //设定时间为100ms
		T0Count = 0;
		P2 = ~(~P2 << 1);//流水灯操作
		if(P2 == 0xFF) P2 = 0xFE;//当P2=0xFF左移都为0xFF，因此需要重置P2为0xFE
	}
}
```

## 完整代码

```C
#include "REG51.h"

void Timer0_Init(){
//TMOD = 0x01;        //定时器模式为0000 0001，可用下两行代替
	TMOD &= 0xF0;				//把TMOD的低四位清零，高四位保持不变
	TMOD |= 0x01;       //把TMOD的最低位置1 ，高四位保持不变
	TF0 = 0;            //中断溢出标志位
	TR0 = 1;            //定时器是否开启
	/*该计数器只能从0计到65535个数，每隔1us计数加1，总共定时65535us*/
	TH0 = 64535/256;    //64535的高八位，使计数器计1ms
	TL0 = 64535%256;    //64535的低八位，使计数器计1ms
	ET0 = 1;
	EA = 1;
	PT0 = 0;
}
int main(){
	P2 = 0xFE;
	Timer0_Init();
	while(1){
		
	}
}

void Timer0_Routine() interrupt 1{
	static unsigned int T0Count;
	TH0 = 64535/256;
	TL0 = 64535%256;
	T0Count++;
	if(T0Count>=100){  //设定时间为100ms
		T0Count = 0;
		P2 = ~(~P2 << 1);//流水灯操作
		if(P2 == 0xFF) P2 = 0xFE;//当P2=0xFF左移都为0xFF，因此需要重置P2为0xFE
	}
}
```

