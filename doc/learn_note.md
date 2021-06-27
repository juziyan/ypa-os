# os_tutorial

## chapter 1
**Goal: Create a file which the BIOS interprets as a bootable disk**

boot要放在一个标准位置（cylinder 0, head 0, sector 0），长度为512个bytes，末尾以0xAA55结尾。

## chapter 2
**Goal: Make our previously silent boot sector print some text**

interrupt service routines (ISRs)

ax寄存器的高位ah写0x0e，表示tele-type mode，要打印的字符写如低位al，调用0x10 video interrupt软中断。

打印abcde生成的bin文件：

b40e **b041** cd10 **b042** cd10 **b043** cd10 **b044**

cd10 **b045** cd10 ebfe 0000 0000 0000 0000

**

0000 0000 0000 0000 0000 0000 0000 55aa

打印ABCDE生成的bin文件：

b40e **b061** cd10 **b062** cd10 **b063** cd10 **b064**

cd10 **b065** cd10 ebfe 0000 0000 0000 0000

**

0000 0000 0000 0000 0000 0000 0000 55aa

## chapter 3
**Goal: Learn how the computer memory is organized**

BIOS放在0x7C00

全局，可以相对偏移

    [org 0x7c00]

1. `mov al, the_secret`
2. `mov al, [the_secret]`
3. `mov al, the_secret + 0x7C00`
4. `mov al, 2d + 0x7C00`, where `2d` is the actual position of the 'X' byte in the binary


|  | original | global |
|----|----------|-----------|
|1.  | &cross; | &cross; |
|2.  | &cross; | &check; |
|3.  | &check; | &cross; |
|4.  | &check; | &check; |

## chapter 4
**Goal: Learn how to use the stack**

bp栈底，sp栈顶
栈底的地址最大,所以push的时候sp递减，pop适合sp递增

## chapter 5
**Goal: Learn how to code basic stuff (loops, functions) with the assembler**

汇编语法，封装了一些函数

## chapter 6
**Goal: learn how to address memory with 16-bit real mode segmentation**

| CS | Code Segment |
|----|----------|
| DS | Data Segment |
| SS | Stack Segment |
| ES | Extra Segment |
| FS | General Purpose Segments |
| GS |  |

## chapter 7
**Goal: Let the bootsector load data from disk in order to boot the kernel**

