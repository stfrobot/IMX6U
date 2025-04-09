IMX6U LED驱动

```
.global _start
_start:

ldr r0, =0X020C4068				//寄存器全部置1
ldr r1, =0XFFFFFFFF
str r1, [r0]

ldr r0, =0X020C406C
str r1, [r0]

ldr r0, =0X020C4070
str r1, [r0]

ldr r0, =0X020C4074
str r1, [r0]

ldr r0, =0X020C4078
str r1, [r0]

ldr r0, =0X020C407C
str r1, [r0]

ldr r0, =0X020C4080
str r1, [r0]

ldr r0, =0X020E0068
ldr r1, =0X5
str r1,[r0]


ldr r0, =0X020E02F4			//处理寄存器值
ldr r1, =0X10B0
str r1,[r0]

ldr r0, =0X0209C004
ldr r1, =0X0000008
str r1,[r0]

ldr r0, =0X0209C000
ldr r1, =0
str r1,[r0]

loop:						//循环
    b loop
```

配置流程

1、配置使能时钟

2、找到复用寄存器，找到地址，复用为GOIO

3、找到配置寄存器，找到地址，配置GPIO

4、设置GPIO

5、控制高低电平

![dd8288590ce0f71d52c3045dfc4769d](C:\Users\stf\Documents\WeChat Files\wxid_lxej6vg1en1w22\FileStorage\Temp\dd8288590ce0f71d52c3045dfc4769d.png)

LED0为GPIO3，则为GPIO1_IO03

```
时钟使能
ldr r0, =0X020C4068				//寄存器全部置1
ldr r1, =0XFFFFFFFF
str r1, [r0]

ldr r0, =0X020C406C
str r1, [r0]

ldr r0, =0X020C4070
str r1, [r0]

ldr r0, =0X020C4074
str r1, [r0]

ldr r0, =0X020C4078
str r1, [r0]

ldr r0, =0X020C407C
str r1, [r0]

ldr r0, =0X020C4080
str r1, [r0]
使能所有时钟CCM_CCGR0-CCM_CCGR6，其实只用使能CCM_CCGR1的位27-26为1
```

```
复用为GPIO
ldr r0, =0X020E0068
ldr r1, =0X5
str r1,[r0]
配置IOMUXC_SW_MUX_CTL_PAD_GPIO1_IO03（Address: 20E_0000h base + 68h offset = 20E_0068h）
低4位为0101
```

```
配置GPIO口功能
ldr r0, =0X020E02F4			//处理寄存器值
ldr r1, =0X10B0
str r1,[r0]
配置IOMUXC_SW_PAD_CTL_PAD_GPIO1_IO03（Address: 20E_0000h base + 2F4h offset = 20E_02F4h）
0001000080110000
```

```
配置输入输出
ldr r0, =0X0209C004
ldr r1, =0X0000008  （1000）
str r1,[r0]
配置GPIO direction register (GPIO1_GDIR)（Address: Base address + 4h offset）
位3设置为output
```

```
配置输出01
ldr r0, =0X0209C000
ldr r1, =0
str r1,[r0]
配置GPIO1_DR（0X0209C000）
位3写入0，输出低电平，led亮
```

