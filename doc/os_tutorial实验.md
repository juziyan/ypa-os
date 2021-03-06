## 环境安装
- Ubuntu-20.04
- nasm
- qemu
## os-tutorial-01实践

```
; Infinite loop (e9 fd ff)
loop:
    jmp loop 

; Fill with 510 zeros minus the size of the previous code
times 510-($-$$) db 0
; Magic number
dw 0xaa55 
```
编译：`nasm -f bin boot.asm -o boo.bin`
运行：`qemu-system-x86_64 boo.bin`
输出：
![image](https://user-images.githubusercontent.com/76890712/122787328-cdfb2a00-d2e7-11eb-9e4a-cc1265c9d4cb.png)

ps:
生成的bin文件可以通过hexdump查看
- 安装hexdump：`sudo apt-get install libdata-hexdumper-perl`
- 查看bin文件内容：`hd ./boo.bin`结果如下
```
yan@ubuntu:~/ypa-os$ hd ./boo.bin
00000000  eb fe 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa  |..............U.|
00000200
```
*生成的bin文件和demo中的bin文件有差异，但是输出结果是相同的，可能是由于编译器的原因*
#### 知识点
1. 汇编语言中的符号$表示当前代码的偏移地址，$$符号表示代码的起始地址
## 02打印hello
```
mov ah, 0x0e ; tty mode
mov al, 'H'
int 0x10
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
int 0x10 ; 'l' is still on al, remember?
mov al, 'o'
int 0x10

jmp $ ; jump to current address = infinite loop

; padding and magic number
times 510 - ($-$$) db 0
dw 0xaa55 
```
编译：`nasm -f bin boot.asm -o boot.bin`
运行：`qemu-system-x86_64 boo.bin`
输出：
![image](https://user-images.githubusercontent.com/76890712/122791168-94c4b900-d2eb-11eb-850d-d45d4414f8c2.png)

#### 知识点
1. BIOS interrupt call:https://en.wikipedia.org/wiki/BIOS_interrupt_call
![image](https://user-images.githubusercontent.com/76890712/122793599-f9811300-d2ed-11eb-97e3-7ef0581ed22d.png)

## 03 关于绝对偏移地址和相对偏移地址

![image](https://user-images.githubusercontent.com/76890712/122943386-8e951200-d3a9-11eb-883c-135041924e98.png)

## 04 关于stack的概念
- bp，sp
- 栈顶移动方向
- pop,push

## 06 关于段偏移地址
- 寻址方式

## 07 关于大小为512字节的主引导扇区
处理器在加电或复位时，如果启动设备为硬盘，则ROM-BIOS会尝试读取硬盘的0面0道1扇区（MBR），读取的大小为512字节，ROM-BIOS会将其加载到物理地址0x7c00处，并校验其是否有效；一个有效的主引导扇区的数据最后两个字节应为0x55和0xAA；如果有效，则跳转至0x7c00处继续执行。



