# 1. 环境搭建
存储位置位于E盘, 创建新的文件流程是 ，在命名部分记得把尾缀 .c写上，不然属于是c++语言.
运行代码的操作，首先在vscode终端中找到，然后使用
``` Terminal
gcc 文件名.c -o 文件名  //为源文件生成可执行文件
./文件名    //运行生成的可执行文件
```
进入虚拟机的办法：`wsl`, 然后就可以执行`sudo`指令
虚拟机密码：`15081139320Hzs`
## 2. Hello World代码结构
```  C
#include<stdio.h> //标准输入输出位置，就是控制台，include表示提前找到文件的文件位置

int main() //程序主入口，int表示整个程序的运行结果是一个整数
{
	printf("Hello world");
	return 0;//程序运行结果，程序正常结束。
}
```
### 2.1 流程分析
首先用户把代码写好，写为尾缀是c的文件，其次运行的过程中， vs会帮助我们把写出来的代码翻译为计算机认识的语言，然后整合运行，运行后的文件，在项目文件流程里的
E:\学习\第二学年第一学期\algorithms\HelloWorld\HelloWorld\x64\Debug 会生成运行文件。

# 3 核心语法
### 3.1注释
1. 单行注释使用//
2. 多行注释使用 /*  */
3. 多行注释添加 Ctrl K, Ctrl C ;Ctrl K, Ctrl U可以取消注释
## 3.2 变量赋值
1. 数据类型 变量名 = 数据值 ；(定义+赋值)
2. int a =10;
3. char a ='c' ;
## 3.3 字符串String
空字符 \0用于标记字符串的结束，字符串通过char 后面添加[]进行定义
```C
char greeting[] = "Hello"; //这种情况下编译器自动分配存储空间，6字符，五个字符以及一个空字符
char last_name[20]; //这种是手动分配内存空间，当定义空字符串的时候必须定义它的大小。定义空字符串
```
数组内实际存储的有 {'H', 'e', 'l', 'l', 'o', '\0'} \0表示字符串的结束

## 3.4 小数类型：
一共有两种，一种是float（精确度为小数后6位），另一种是double（精确度为小数后15为）
```C
//定义float类型
float a = 3.14F;  //一般默认情况是double类型，因此在末尾添加f来保证是float类型
printf("%.2f\n",a);  //在%和f之间添加.2来表示保留两位小数

//定义double类型
double b = 1.78;
printf("%.2lf\n",b) //这里的double类型是在%和lf之间加.2
```

## 3.5 输出常量
1. printf(输出内容的最终样式，填补的内容), 占位符  
	1. 整数 %d
	2. %6d 假如宽度不满足6位，则会在前面补充空格
```C
char letter = 'J'; 
printf("Character as number: %d",letter); //这种情况下输出结果为字母J的ASCII编码
```


> [!NOTE] 常用占位符
> 1. `%f`小数   输出的是十进制浮点数，默认保留6位小数
> 2. `%d`整数
> 3. `%e` 以指数形式，科学计数法来输出浮点数 3.140000e+00
> 4. `%c`字符 ,。假如给出的是ASCII的值，那么输出结果是这个ASCII码对应的符号
> 5. `%s`字符串 
> 6. `%x` 是 **格式化占位符**，用于以 **十六进制格式**输出整数。
> 7. `%zu`指示字符大小   配合sizeof()使用
> 8. `% p`指示地址 
	例子： printf("我的身高是: %f\n ", 1.86)
	printf(“女朋友名字是: %s，年龄: %d”, “臭萱”，18)
	例子：printf("Address of a is %p\n", &a);
	

> [!NOTE]
> 把char转化为int类型则会输出其浮点数
> 把int转化为char类型则会输出其ASCII码


## 3.6 输出字符
1. `putchar()`，只能输出单个字符，无需占位符。<font color="#76923c">如果不换行的话会一直在一行内输出</font>
```C
for (char c = 'A'; c <= 'Z'; c++) { 
putchar(c); // 输出：A 到 Z 
}
putchar('\n'); // 换行
```

2. `printf()`， 需要使用占位符来区分，因为可以输出不同类型的数据
3. `printf()`还可以确定字符或者字符串的宽度，注意，这个空格是会靠在最右侧的。
![[Pasted image 20250113222251.png]]
4. `puts()`会在打印完自动添加一个换行符
5. `sprintf()`，将输出存储到数组中，而不是直接打印到控制台，以便未来使用。[sprintf() Function | C Programming Tutorial](https://www.youtube.com/watch?v=Ps-KajI0asw)
```
char out_str[20];  //先定义一个大小为20的数组
int value = 12;
sprintf(out_str ,"The value is: %d", value);//存储到数组中
puts(out_str);
```

## 3.7 输入数字，文本

#### 3.7.1 scanf( )
> [!NOTE] `scanf( )`特点
> 它接收的是一个地址！

```C
#define _CRT_SECURE_NO_WARNINGS //有时候会说scanf不安全，添加这个语句
#include <studio.h>
.....//省略代码
int a;
int b;
scanf("%d",&a);  
//当录入多个数据的时候，中间是空格，则录入的时候必须是空格,不能是逗号
scanf(" %d %d", &a, &b); //注意这里使用了空格消耗掉回车键

printf("Enter two digits:");
scanf("%d%d",&a,&b);
printf("You entered: %d,%d\n",a,b);
```

记得添加取地址运算符 & , 因为 `scanf` 需要知道变量在内存中的位置。
> - `scanf()`同时可以处理多个数据输入，因为 `scanf` 在读取数据时会自动跳过空白字符，包括空格、换行（`\n`）、Tab 等。
> - 这意味着用户可以通过任意数量的空格或回车分隔输入数据。

> [!STAR] scanf( )小细节
>1.  注意，scanf( )内部的内容需要和键盘录入的内容保持完全一致才可以，否则程序不能正确运行。[核心语法-21-练习2-我的年龄_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=27)
> 2. 当多次需要键盘录入的时候，用户输入完第一次后的回车键`\n`会被程序存储，给到下一个输入，想要避免的话有以下解决方式。 1每次`scanf()`之后都加入一行`getchar()`, 2在第二个要输入的之前添加一个空格。
> 3. scanf()内部无需添加换行符，否则程序不执行
> 4. 成功接受到目标值scanf会返回1， 不符合返回0， 什么都没有返回-1



2. 输入字符串
如果定义了一个字符串，
> `char str[100];` 
   `scanf("%s",str);` 这种情况下不需要加&地址，因为本身就是数组的首元素的地址，即指向数组首元素的指针
#### 3.7.2 fgets()
适合读取一整行字符串，可以保留输入中的空格以及换行符。因此整行的内容都会被存储在字符串中。
```C
char str[100]; 
printf("请输入一行文本："); 
fgets(str, 100, stdin); // 从键盘输入一行最多100个字符 
//这里会输入到str中
printf("你输入的文本是：%s\n", str);
```

### 3.7.3 getline()
适用于动态分配大小，然后在进行读取
```python
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *line = NULL;   // 存储输入的字符串
    size_t len = 0;      // 缓冲区大小
    _ssize_t read;        // 存储 getline() 返回的字符数

    printf("请输入文本（按 Ctrl+D 结束）:\n");

    while ((read = getline(&line, &len, stdin)) != -1) {
        printf("你输入的内容是: %s", line); //getline可以读取到\n
        printf("当前缓冲区大小: %zu 字节\n", len);
    }

    printf("\n检测到 EOF，输入结束。\n"); //当按下Ctrl+D之后会自动退出

    free(line);  // 释放动态分配的内存，避免内存泄漏
    return 0;
}


```
## 3.8 常量的定义
```C
#define PI 3.14  //也可以在这里定义，这种定义方式后期还可以更改

#undef PI  //更改这种定义
#define PI 3.14159

const int MAX_USERS = 100;  //定义后，不可使用指针等更改
```

## 3.9 类型转化
1. 字符转化为数字类型：字符 - '0'

---

# 4. Data and memory
## 4.1 Bits, Bytes, characters and memory addressing.
**Bit** has a value of either 0 or 1. 
**Byte** is a unit of digit information mostly consist 8 bits.
**Hexadecimal** (十六进制) : 每一个十六进制数字对应一个四位的二进制数。
	比如十六进制的0 可以用2进制的 0000表示
	十六进制的 9 可以用二进制的1001表示
每四位二进制的数都可以用1个16进制数字表示

## 4.2 Representing integers and floats

1.  比如 unsigned int a ;
 我们没有给他规定符号， 因此a 只能存放非负数的整数； 通常情况下的是4字节，也就是32位。
2. int 自身是有符号的，正数用二进制表示，负数用二进制的补码表示，对于int类型，32位中有一位用于存放符号，剩下31位表示数字。由此int的范围是    **$-2^{31}$ 到 $2^{31}$ - 1**
3.  long int 是8字节，也就是64位
4. float 的结构（single - precision - floating point）
			Sign | Exponent | Mantissa
			--1 Bit- ---8 Bits -----23 Bits--
比如将 85.125转化为浮点数
1. 首先转化85，85 = 1010101
2. 然后转化0.125，$0.125*2 = 0.25$，整数0，$0.25 *2 = 0.5$， 整数 0， $0.5* 2= 1$，整数1， 因此是 001
3. 1010101.001，转化小数点到 $1.010101001 * 2^6$,之后我们丢弃第一个 1，补齐至23位。
4. 010 1010 0100 0000 0000 0000
5. ==转化为IEEE 754 单精度浮点数==：
		尾数 010101001
		指数 实际指数为6（次方），单精度浮点数偏置值是127（固定值），因此是 6 +127 = 133， 转化二进制为 10000101
		符号位 0（正数0，负数-1）
## 4.3 Pointers
指针是存储内存地址的变量。本身也属于变量，因此定义的方式类似于变量。指针定义的数据类型要与它指向的变量类型保持一致。[指针-02-指针的第一个作用_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=115) [String_literals_character_arrays.pdf](https://moodle.lut.fi/pluginfile.php/2453169/mod_label/intro/String_literals_character_arrays.pdf?time=1737457466526)


> [!NOTE] 指针小注意
> 1. 不能修改指针指向的内容。
![[Pasted image 20250131233515.png]]
> 1. 本质上指针指向的是字符串的首地址，虽然指针不可以修改，但是可以调用
> ![[Pasted image 20250131234021.png]]    // 可以看到str2本来指向的是字符串的首字母地址a，现在+2后指向首字母c了
```C
 数组名是常量指针，指向固定的地址，不能让它赋值指向别的内存；但是可以给指针赋值让它指向别的内容
char arr[] = "Hello";
arr = "World";  // ❌ 错误

char *ptr = "Hello";
ptr = "World";  // ✅ 合法，使得整个指针指向另一个值
*ptr = 'A'    //此时为Aorld，解复用只更改指针指向的字符
//无论字符大小，sizeof()一个指针的大小都是8，因为sizeof只会显示指针的大小，而非数据长度，在函数内部就会退化
char *str = 'ab';
printf("%zu\n",sizeof(str));  //结果是8在64位操作系统，而非3.
```

![[Pasted image 20250429165416.png]]  
只要我想对指针所指向的内容进行修改了，那么我就要使用解复用。

> [!NOTE] Sizeof与strlen的区别
> `sizeof`获取的是**数组总大小**（单位：字节），和实际填了多少内容无关；如果数组作为**函数参数传进去**，就**退化成指针**，`sizeof`只返回指针大小。
`strlen`计算的是**字符串实际内容长度**（遇到`\0`结束），才是用来获取**实际有效字符数量**的方法
  如果是int类型的数组，会提前把个数传入到函数中，因为函数内部没有办法获得数组的个数；
  办法就是`sizeof(nums)/sizeof(nums[0])`,或者采取计数器的形式
  char类型的数组，可以不传入，后面使用strlen获取，因为strlen只能获得字符串的长度。



2. 指针的两个功能分别是①查询数据，②存储数据。
``` C语言
int a = 10;
int* p = &a;   这里的*标记P变量，说明这个是指针；这里&符号表示获取a的地址。
//int* p =500;这样赋值是错误的
printf("%d\n",*p); 这里是用指针进行查询

*p = 20; 使用这个指针来存储数据
printf("%d\n",*p);
```
①指针的进阶使用：交换两个值，需要交换内存地址所指向的，这样才能真正交换
``` C
#include<stdio.h> //标准输入输出位置，就是控制台，include表示提前找到文件的文件位置

void swap(int* num1, int* num2);
int main() //程序主入口，int表示整个程序的运行结果是一个整数
{
	int a = 10;
	int b = 20;

	printf("Before the exchange values are %d , %d\n", a, b);

	swap(&a,&b);
	printf("After the exchange values are %d , %d\n", a, b);

	return 0;
}

void swap(int* num1, int* num2) {  //这里的*表示接收参数是指针
	int tem = *num1;  //取出地址里的值，存储到int类型中
	*num1 = *num2;  //这里*的意义是解引用，访问指针内存存储的值。
	*num2 = tem;

}
```

拓展： 交换数组

```C
#include <stdio.h>

void swap_arrays(int *a, int *b, int len) {
    for (int i = 0; i < len; ++i) {
        int temp = a[i];   // 相当于 temp = *(a + i)
        a[i] = b[i];
        b[i] = temp;
    }
}

int main() {
    int arr1[] = {1, 2, 3};
    int arr2[] = {4, 5, 6};

    int len = sizeof(arr1) / sizeof(arr1[0]);

    swap_arrays(arr1, arr2, len);

    // 输出结果验证交换
    for (int i = 0; i < len; ++i) {
        printf("arr1[%d] = %d, arr2[%d] = %d\n", i, arr1[i], i, arr2[i]);
    }

    return 0;
}

```

当使用指针的适合，假如指针存在一个函数中，那么当函数结束了，这里存储的值也会消失，为了避免消失，可以使用static变量。[指针-03-第一个作用的细节_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.player.switch&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=116)
②指针进阶使用，求数组最大值最小值。[指针-04-指针的第二个作用_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.player.switch&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=117)
③区分函数的计算结果以及计算状态 [指针-05-指针的第三个作用_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=118)
指针的运算，根据数据类型不同，每一次对指针+1，实际指针的步长也不同。[指针高级-01-指针的运算_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=119)

3. 二级指针，指向指针的指针（指针本身也是一个变量）[指针高级-05-二级指针和多级指针_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=123)
比如一级指针是 int* p，指向这个指针的指针是 int ** p.
# 5. Structure of program.
## 1. `Preprocessor section
4. 预处理器部分包含所有使用到的头文件，通知系统将头文件与数据库连接
```
#include <stdio.h>  //标准输入输出库
#include <string.h>  // 字符串操作库
```
5. `#define`指令
```
#define PI 3.14159       //这样可以避免代码多次重复定义常量值
```
## 2. main( )
作为程序的入口点，由main( )开始执行程序
```
int main(void)   //不接受任何参数
int main()    //参数为空，功能类似于 int main(void)
```
通常情况下`return`的值为0表示程序成功运行，非零表示由错误或者终止。


# 6. Conditional Statement

## 6.1 if循环
``` C
if ((rowA == rowB) && (numA-numB ==1  ||numA - numB == -1)){
	//内容
}else if{  //相当于elif
	//内容
}else{
	//内容
}
```
## 6.2 Switch 语句
```C
// w2_2
	float a;
    int formula;
    float result;
    printf("How much electricity did you use this month?\n");
    scanf("%f",&a);
    formula = (int)a / 500;

    switch (formula) //表达式， 这里是执行，而不是判断
    {
    case 0:
        result = a*0.18;
        break;
    case 1:
        result = 500*0.18 + (a-500)*0.21;
        break;
    default:
        result = 500*0.18 + 500*0.21 + (a-1000)*0.25;
        break;
    }
    printf("Total electricity bill this month is %.2f EUR.\n",result);
```

> [!NOTE] 细节
> 1. 表达式中的值必须是字符或者整数， 不能是小数，字符串等等
> 2. case 不允许重复，并且值只能是字符，整数，不能是变量
> 3. default部分甚至可以省略
> 4. 如果case 里面的break 忘了写，那么会造成case穿透，后续代码全部运行。[流程控制语句-09-case穿透_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=55)，视频中也有case穿透的应用。

> [!NOTE] switch和if的区别
> switch 用在有限的case， 假如是一个范围，或者情况比较多，建议使用if
> switch的执行效率比if要高


## 6.3 循环结构
### 6.3.1. For 循环：  
循环结束后，i不能被访问；**知道循环次数**，或者范围
```C
for (int i = 1; i <= 100; i++){  //循环开始条件；结束条件；条件控制语句
	//循环内容
}
```### 6.3.2. While循环：
// 及时循环结束，i也可以被访问；**不知道循环次数**，只知道循环结束条件
``` C
int i = 1;
while(i <=5){
printf("%d\n",i)
i++
}
```
### 6.3.3. do...while 循环（少见）：
先执行代码，在进行判断
``` C
do{
	//循环体
}
while(condition)
```
### 6.3.4. 无限循环
``` C
for (;;){
	printf("")
}

while(1){   //1代表True，这样表示一直循环
	printf("")
}

do{
	printf("")
}while(1)
```
### 6.3.5 break和continue
6. `break` 只能写在 switch 或者在循环中，不能单独存在；结束**整个**循环
7. `continue` 结束本次循环，继续下次循环 [循环高级-03-continue_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=74) 结束**本次**循环

# 7. 算术运算
8.  % 用来取余
9. 后置递增和前置递增 i++和++i
```C
int i = 5;

    int x = i++;
    printf("Post-increment:\n");
    printf("x = %d, i = %d\n", x, i);  // x=5,i=6 ，先使用i赋值给x，再将i+1

    printf("Pre-increment:\n");
    int y = ++i;
    printf("y = %d, i = %d\n", y,i); //y=7,i=7  先+1，在赋值
```
10. 0 代表False， 剩下的任意不为零的都代表True
11.  Boolean Operation: 
	1. && 类似于and
	2. ||类似于or
	3. ! 类似于 not
# 8. <string.h>文件
提供了操作字符数组的各种函数，包括 复制，比较，搜索，确定长度等
## 8.1 strlen():
返回数组长度，不可以计算int类型的数组里面有多少个int

`strlen` 是专门用来**计算字符串长度**的函数。

**详细点说**：

- 它接受一个 **`char*` 类型**（字符指针，或者说字符数组）的参数。
    
- 然后它**从你传进去的地址开始，一直往后读字符**，直到遇到第一个 `'\0'`（空字符，ASCII码为0），
    
- 最后返回**遇到 `'\0'` 之前字符的数量**，不算 `'\0` 本身。

## 8.2 strcpy():
将一个数组复制到另一个
```C
char str1[] = "Hello";
    char str2[10];
    strcpy(str2,str1);
    printf("Copied string: %s\n",str2);
```
## 8.3 strcat(): 
将一个字符串连接到另一个的末尾
```C
char str1[] = "Hello";
    char str2[] = "World";
    strcat(str1,str2); 
    printf("String:%s\n",str1);
```
## 8.4 strcmp(): 
按照字典顺序进行比较两个字符串，负值表示第一个字符串的顺序小于在字典中第二个字符串的顺序。注意必须是char类型
```C
char nums[] = "123";
    char nums2[] = "456";
    int result = strcmp(nums,nums2);
    printf("%d\n",result);
```

## 8.5 strchr()
意为 在string 中查找 char，返回一个指针，如果找不到那就返回null
```C
char str[] = "C Exam";
    char c = 'C';
    char *p;
    p = strchr(str,c);
    printf("Show at %s\n",p);
```

## 8.6 strstr()
同样的，返回str2出现位置的指针，获取到的是地址。下面是一个实现查询位置的范例
由于 result指针相当于指向str2出现初始位置的指针，即`&str1[8],而str1则代表了&str1[0]`因此两个相减即是偏移量。
```C
    char str1[] = "This is an example";
    char str2[] = "an";
    char *result = strstr(str1,str2); //这里实际上获得是一个地址

    printf("The sub_string %s was found in position %ld\n",result - str1);
```

扩展：如何找到以子串的形式分割
```
void split_substring(char *str, char *sep){
    char *pos;
    while ((pos = strstr(str,sep))!=NULL){
        *pos = '\0';  //这里需要对地址内的值进行修改了
        printf("%s\n",str);
        str = pos + strlen(sep);
    }
    printf("%s\n",str);
}
int main(){

    char str1[] ="HelloabWworld;";
    char str2[] = "ab";
    split_substring(str1,str2); 
}
```
![[Pasted image 20250429152729.png]]
## 8.7 strtok()

`strtok(char *str, const char *delim)`切割字符串，用于将str按照delim中任意字符进行切割.
后续调用的时候传入 null即可，注意delim其实是一个字符集合，出现一个就会切割

```C
char str1[] = "This is an example";
    char str2[] = " ";
    char *token = strtok(str1,str2);

    while(token!=NULL){
        printf("%s\n",token);
        token = strtok(NULL,str2);
    }
```
# 9. 函数

void 函数名(形参)   //假如需要使用返回值，那记得return，假如不需要返回值，那么记得void。
{                               //也可以先写void，然后根据return的类型在把void进行修改。
	函数体；
	return 值；
}
	形参 formal parameters 
> [!NOTE] 注意事项
> 自定义函数需要在main的上方进行定义，而不能放在下面。

假如想要修改字符串，那么首先要将其定义为字符串组，而不是字符串字面量，因为字面量存储在只读结构中不可以被修改。[c++ - Difference between char* and char[] - Stack Overflow](https://stackoverflow.com/questions/7564033/difference-between-char-and-char) 数组内容可以修改，被指针指向的字符串常量不可更改
```C
void modifyString(char str[]){
	str[0] = 'Z';
}

int main(){
	char str[] = "Original String.";
	printf("Before:%s\n",str);  //Original String
	modifyString(str);
	printf("After:%s\n",str);  //Zriginal String
	return 0;
}
```


> [!NOTE] 改变变量的值，但是函数使用void
> 在 C 语言中，函数参数默认都是按值传递的，也就是说函数内部接收到的是实参的一个副本。如果直接传入变量的值，函数内部对该副本的修改不会影响原始变量。
> 
而当你传入变量的地址时，你传递的是一个指针，这个指针保存了原始变量在内存中的位置。函数内部通过这个指针解引用（即使用 `*` 操作符），就可以直接访问和修改内存中该位置存储的值。这样，函数内部对该值的修改就直接作用于原始变量，从而实现了在函数内部改变外部变量的值。

## 9.1 格式转化
12. 隐式转化
13. 显示转化 ：(int) float_variable  //强制把浮点数转化位int

## 9.2. 数组
14. 定义： 数据类型 数组名[长度] = {数据值，数据值} [数组-08-数组常见的两个小问题](https://www.bilibili.com/video/BV1Xa4y1k7LU?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=102)
假如访问一个不存在的索引，那么会报错 ：segmentation fault。
字符串数组和其他数组不同之处在于，字符串数组以'\0'结尾，因此可以直接printf()

> [!NOTE] 数组作为函数的参数传递
```C
> 错误做法：（正确的做法应该是把size大小一同传递过去，函数内数组退化为指针）
> void print_size(int A[]){
> int elements = sizeof(A)/sizeof(A[0]); //总共字节大小/一个字节的大小
> printf("....")
> }
> int main(){
> int array[] = {1,2,3,4,5};
> print_size(array); //这么做结果是错误的，因为传递到函数的array只是数组的首地址，
> }
```
![[Pasted image 20250207234000.png]]
15. 如果想要定义一个字符串的数组，那么最好使用带有指针的（不可修改）
```C
char *names[] = {  //这里names数组是存储的一些指针
"Alice",
"Bob",
"Char"
};  //注意字符串是不可更改的，但是可以将指针指向别的值
names[0] = "Michael"; // Alice本身不可修改，但是可以把整个修改 names[0] 使其指向新的字符串字面量
```

![[Pasted image 20250429193642.png]]
16. 多维数组
```C
int main(){
int matrix[2][3] = {{1,4,2},{3,6,8}};  //两行三列的意思
matrix[1][2] = 88;
for (int i =0;i<2;i++){
	for (int j = 0;j<3;j++){
	printf("%4d",matrix[i][j]);}
	printf("\n");
}
}
```
17. 2维字符串数组（可修改）
```C
char names[3][20] = {"Alice", "Bob", "Charlie"};  //一共存储三个字符串，每个最大20字符
names[1][0] = 'R';  // 把 "Bob" 改成 "Rob"
```
如果使用指针字符串，那么指针字符串只存在只读区，无法对内部元素进行修改，

## 9.3.stack 栈 和 heap 堆
遵循后进先出，最后压入的最先弹出。 主要应用场景有：在函数内部声明的变量，每次调用函数的时候，递归调用等
Heap： 动态分配内存，在程序运行的时候按需分配。内存需要管理员手动释放，否则造成内存泄露。比如malloc

 ## 9.4. 动态内存分配

首先思考一个问题。假如想要返回一个数组，一定要使用malloc，因为return一个数组实在栈上，这个栈在函数结束后自动释放，因此返回的将是一个野指针。所以采取heap堆，malloc。

> [!NOTE] 常用函数
> malloc： 申请动态空间，需要是连续的
> calloc: 申请空间+数据初始化
> realloc修改空间大小
> free释放空间

![[Pasted image 20250501182856.png]]

1. 首先导入#include<stdlib.h>
```C
//假如我们要申请一段连续的地址
int* p = malloc(10*sizeof(int));  //这篇空间的首地址是p，动态划分了100个空间来存储int类型
//实际上malloc是一个void类型的指针， 但是我们通过强转把它变成了int类型
int size = 10;  //一般提前定义出来，因为sizeof(p)出来的是指针大小
if(p == NULL){  //可以查看是否分配成功
printf("Memory allocation failed!")
}

//第一种赋值方式
*p = 10;  //给首地址赋值
*p+1 = 20; //给偏移量为1的地址赋值

//第二种赋值方式
p[1] =20; 
printf("%d",*p); //解引用，进行打印。
```
2. 现在我发现给我分配的空间有点不够了，我想把它改成20个int大小的
```c
int * pp = realloc(p,20*sizeof(int));  //首先放一个指针，然后放上新换上的大小

free(pp);  //记得释放
```

[[动态分配存储字符串容器]]
## 9.5 二叉搜索树
![[Pasted image 20250309162729.png]]  
[Binary search tree - Implementation in C/C++](https://www.youtube.com/watch?v=COZK7NATh4k&t=6s)

# 13. 文件
1. 转义字符: \的作用是吧这个符号原本的含义进行改变。
2. 打开文件` fopen()`
3. 读数据 `fgetc()`  一次读取一个,fgets()一次读取一行,fread()  
4. 关闭 `fclose()`

## 13.1  文件的读写模式

> [!NOTE] 常见的读写模式
> r只读模式
> w只写模式
> a追加模式

```C
char filename[50];
printf("Enter the file name:\n");
scanf("%s",filename);
FILE* file = fopen(filename,"r"); //打开文件，首先是一个指针，可以指向文件绝对路径下的地址
```
## 13.2 读取文件中的内容
1. 使用`fgets`一次读一行，读不到返回null
```C
char arr[1024];  //读取的内容会存储在缓冲区内，一个数组
char* str1 = fgets(arr,1024,file)  //一次读取一行，以一个换行符为准
fclose(file)
```
2. 使用`getline()`动态确定缓冲区大小，适合于长度不确定的行
其中，line是字符串的指针，`read = getline(&line,&len,stdin)` 这时候stdin用于键盘输入
如果是`getline(&line,&len,fp)`其中fp是文件指针，那么就是读取文件内容。==如果读取不到就返回-1；==  当用户使用Ctrl +D 的时候自动停止。
```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *line = NULL;      // 指向读取内容的指针（会动态分配）
    size_t len = 0;         // 初始化长度为 0，getline 会自动分配
    ssize_t read;           // 存储读取的字符数（含换行）

    printf("Please enter a line of text:\n");

    // 从标准输入读取一行
    read = getline(&line, &len, stdin);
    if (read != -1) {
        printf("You entered (%ld chars): %s", read, line);
    } else {
        printf("Failed to read line.\n");
    }

    // 释放动态分配的内存
    free(line);

    return 0;
}


//这里是适用于文件打开的
FILE *fp = fopen("input.txt", "r");
if (fp == NULL) {
    perror("File open failed");
    return 1;
}

while ((read = getline(&line, &len, fp)) != -1) {
    printf("Line: %s", line);
}
fclose(fp);
free(line);


```

## 13.3 写入文件内容
### 13.3.1 `fprintf()`
适用于文件的格式化输出，允许使用%d,%s等格式化后写入
```C
FILE *fp = fopen("output.txt", "w"); // 以写模式打开文件
int age = 25; char name[] = "Alice"; // 向文件写入格式化数据 
fprintf(fp, "姓名: %s, 年龄: %d\n", name, age);
fclose(fp);
```
### 13.3.2 `fputs()`
不支持格式化，直接写入比fprintf更快
```C
FILE *fp = fopen("output.txt", "w");
char text[] = "Hello, World!\n"; // 向文件写入字符串 
fputs(text, fp); 
fclose(fp);
```
### 13.3.3 `putc()`
适用于向文件写入一个字符
```C
FILE *fp = fopen("output.txt", "w");

char ch = 'A'; // 写入单个字符 

putc(ch, fp); 
putc('\n', fp); 

fclose(fp); 

```

# 14.模块化编程
## 14.1 Struct 结构体
```C
//定义结构体
struct Person{
	char name[50];
	int age;
	float height;
};
//初始化结构体
struct Person person2 = {"Alice", 20, 1.70f};

//也可以使用别的办法进行赋值（声明<string.h>)
strcpy(book1.title, "The C Programming Language");
//访问结构体
printf("Name: %s\n", person1.name);
printf("Age: %d\n", person1.age);
printf("Height: %.2f\n", person1.height);

```
## 14.2 typedef 
可以创建别名这和一样
```C
struct Book { 
char title[50]; 
char author[50]; 
char publisher[50]; 
int year; 
};

typedef struct { 
char title[50]; 
char author[50]; 
int year; } Book; 

int main() {  
Book library[3] = { // 定义一个长度为 3 的 Book 数组，名为 library，并进行初始化
{"The C Programming Language", "Brian Kernighan", 1978}, 
{"How to Survive a Zombie Apocalypse", "Max Brooks", 2003}, 
{"Python Crash Course", "Eric Matthes", 2015} 
};
```
访问结构体的内容，除了可以使用`结构体.`之外, 还可以使用`结构体地址->`的方式.
```C
struct Person { 
char name[50]; 
int age; };

int main() {
struct Person person1 = {"Alice", 30}; //定义一个结构体
struct Person *ptr_to_person;   //定义一个指针
ptr_to_person = &person1; // 进行赋值
//通过指针访问结构体成员
printf("Name: %s\n", ptr_to_person->name);  
printf("Age: %d\n", ptr_to_person->age);

printf("Name: %s\n",(*ptr_to_person).name);
printf("Age: %s\n",(*ptr_to_person).age);
```
## 14.3 链表

首先要定义一个节点，然后定义一个链表的整体结构list  

```C
typedef struct node {  //这里的node不要省略，不然后续没有办法指向自己
    int data;
    struct node *next;  //一个结构体需要指向自己
} Node;  
//
typedef struct{   
	Node *head;  //整个链表 = 一串 Node 节点按 next 连起来的链条
}List;           //List = 抓住链条开头的手（即指向第一个 `Node` 的 `head` 指针）

//创建一个空列表
List *create(){
	List *L  = (List*)malloc(sizeof(List));
	L->head = NULL;  //L 的head就是代表了链表的起点
	return L;
}
//插入一个元素
void (insert List*L , int key){
	Node* pNew = (Node*)malloc(sizeof(Node));
	pNew->data = key;
	pNew->next = L->head;  //把原先头结点的值赋值给新节点的next
	L->head = pNew;
}
```

一个简单的链表结构如下
```C
#include <stdio.h>
#include <stdlib.h>  //用于动态创建和释放节点

// 定义链表结构体
typedef struct student {
    int score;
    struct student *next;
} LinkList;

int main() {
    // 创建链表的头节点
    LinkList *head = (LinkList *)malloc(sizeof(LinkList));
    head->score = 90;
    head->next = NULL;  // 当前链表只有一个节点

    printf("第一个节点的分数: %d\n", head->score);

    free(head);  // 释放内存
    return 0;
}

```
这是一种以更高层次创建的形式，这样操作不拘泥于head指针。
```C
typedef struct Node {
    int data;
    struct Node* next;
} Node;

typedef struct {
    Node* head;  // 链表的头指针
} List;

List* create() {
    List* L = (List*)malloc(sizeof(List));
    L->head = NULL;
    return L;
}
```
定义一个头插法的函数 [链表的头插法，数据结构与算法完整代码动画，考研408 期末考试数据结构_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Vw41147xe/?spm_id_from=333.337.search-card.all.click&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61)  注意所有的插入节点都使用`a->next = b` 表示“a 指向 b”，以及尾插法[链表的尾插法 完整代码动画版，数据结构与算法。附在线数据结构交互式工具_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ma4y1z7jf?spm_id_from=333.788.videopod.sections&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61) 
```C
void insert(List *L, int key){
Node *pNew; //声明一个指向新节点的指针
pNew = (Node*)malloc(sizeof(Node));  //分配内存

pNew-> data = key; //把key存入到新节点的data字段
pNew-> next = L-> head; //把新节点链接到原链表
L-> head = pNew; //然后再把链表头节点更新为新节点
}
```
删除一个值[删除链表中的结点 数据结构与算法，完整代码动画版，附在线数据结构交互式工具_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ra4y167VU/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61)
```C
void delete(List *L,int value){
	if(L->head ==NULL){
	return;
	}
	Node *current = L->head;
	Node *previous = NULL;

	if(current->data == value){
	L->head = current->next;
	free(current);
	return;  }//假如头结点就是要删除的值

	while(current!=null &&current->data !=value){
	previous = current;
	current = current->next;}

	if(current ==NULL){
		printf("Value '%d' not found\n",value);
		return;}
		
	previous->next = current->next;
	free(current);

```
插入节点
```C
//print  the list
	void print_list(Node *ptr){
	puts("Printing...");
	int position = 1;
	while (ptr !=null){
	printf("Node position %d: %d.\n",position,ptr->data);
	position = position + 1;
	ptr = ptr->next;
	}
	}
}
```

翻转链表
```C
void reverse(List *L){
    Node* pre = NULL;
    Node* current = L->head;  //首先要有一个指向头结点的指针
    Node* next = NULL;

    while (current != NULL) {
        next = current->next;  //趁着还没有断开连接，先保存下来current头节点的下一个节点
        current->next = pre;  //在这里改变箭头方向
        pre = current;  //将pre向后移动到current的位置
        current = next; //将current移动到next上,必须先移动pre
    }
    L->head = pre;
}
```
![[Pasted image 20250505164145.png]]

[[链表完整版]]

其中需要注意的点
![[Pasted image 20250505224657.png]]
## 14.4 `valgrind`
可以用来检测是否有内存泄漏，语句为在编译之后输入 valgrind  ./文件名
```
valgrind --leak-check=full ./文件名  
```
这样可以输出具体哪里没有被关闭  [Fix Memory Leaks in C Code with Valgrind](https://www.youtube.com/watch?v=DyqstSE470s)

## 14.5 模块化编程的基础
1. 为每个模块创建一个头文件math_library.h，用于声明模块的接口。
```C
#ifndef MATH_LIBRARY_H
#define MATH_LIBRARY_H
float Abs(float x);
LLU Fact(int n);
float Sqrt(float s);
double Power(float x, int n);  //声明函数接口
#endif // MATH_LIBRARY_H  这些属于是包含保护
```

实现文件 math_library.c
```C
#include"math_library.h" //用户自己定义的用这个

float Abs(float x){
if (x>=0){
return x;
}else{
return -x;
}
}

```
主文件 main_library.c
```C
#include"math_library.h"  、//注意这里是h
#include<stdio.h>

int main(){
float nrl;
printf("Give a decimal number:\n");
scanf("%f",&nrl);
printf("|%.5f| = %.5f\n",nrl,Abs(nrl));
}
```

项目编译
```
gcc math_main.c math_library.c -o math
```
不需要管h头文件即可。

#编译生成目标文件.o [Compiling, assembling, and linking](https://www.youtube.com/watch?v=N2y6csonII4)
![[Pasted image 20250307180616.png]]
```
gcc -c math_library.c -o math_library.o
gcc -c math_main.c -o math_main.o
```
链接目标文件至可执行文件
```
gcc math_main.o math_library.o -o math
```
运行程序
```
./math
```

## 14.6 创建自己的库
[How To Create A Library And Split A Program Across Multiple Files | C Programming Tutorial](https://www.youtube.com/watch?v=x8gsHFBW7zY)

> [!NOTE] 步骤
> 此时假如我们已经 创建好了 math_library.h 以及math_library.c
   接下来把源文件编译为目标文件gcc-c math_library.c-o math_library.o
   使用ar命令创建静态库 ar rcs `libmy_mathlib.a` math_library.o  这里的名字可以自行添加。
   通过gcc math_main.c-L. -lmy_mathlib-o myprogram 使用创建好的库
   
## 14.7 Makefile简介
Makefile 被用于自动化编译，尤其在包含多个源文件的时候Makefile能够提高编译效率。
如果手动编译：
```
gcc -c file1.c
gcc -c file2.c
gcc -c file3.c
gcc file1.o file2.o file3.o -o program
```
每次需修改文件都要重新执行这个操作，而Makefile可以根据.o和.c的时间戳进行对比，观察是否需要重新编译该文件。

比如对于这个文件结构
```
project_directory/
├── math_main.c      # 主程序源文件
├── math_library.c   # 数学库源文件
├── math_main.h      # 主程序头文件
├── math_library.h   # 数学库头文件
├── Makefile         # 构建规则文件

```

这是一个makefile的规则代码，math是最终生成的可执行文件，
- `math_main.o` 和 `math_library.o` 是 `math` 所依赖的两个目标文件（即对象文件）。
- 如果 `math_main.o` 或 `math_library.o` 的时间戳比 `math` 更新，则执行命令重新生成 `math`
- - 使用 `gcc` 命令将 `math_main.o` 和 `math_library.o` 链接（link）在一起，生成一个名为 `math` 的可执行文件。**这里必须是Tab**
- `-o math` 指定生成的可执行文件名为 `math`
```Makefile
math: math_main.o math_library.o
	gcc math_main.o math_library.o -o math
```
接下来是.o文件
```
math_library.o: math_library.c 目标文件：依赖文件
	gcc -c math_library.c      Tab  命令，只编译不链接

```
然后是main.o
```
main.o: math_main.c
	gcc -c math_main.c 将 `math_main.c` 编译成 `main.o` 对象文件
```
最后clean，此命令将删除可执行文件 `math` 和两个对象文件 `math_main.o` 和 `math_library.o`。
```
clean:
	rm -f math math_main.o math_library.o
```

# 15 函数指针
```C
#include <stdio.h>

// 一个普通函数，接受两个 int 参数并返回 int
int add(int a, int b) {
    return a + b;
}

int main() {
    // 声明一个函数指针，匹配 add() 函数的签名
    int (*func_ptr)(int, int);

    // 将函数地址赋值给指针
    func_ptr = add;

    // 使用函数指针调用函数
    printf("Result: %d\n", func_ptr(5, 3));

    return 0;
}

```
另一种
```C
#include <stdio.h>

typedef int (*FuncPtr)(int,int);

int add(int a, int b){
	return a+b;
}

int subtract(int a, int b){
	return a-b;
}

void calc(int a, int b, FuncPtr op){
	printf("Output value is :%d\n",op(a,b));
}

int main(){
	calc(10,5,add);
	calc(10,5,subtract);
}
```

构建结构体来面向对象编程
```C
#include <stdio.h>

// 1. 结构体定义（需前置声明）
typedef struct Rect Rect;

// 2. 函数声明（用于结构体中的函数指针）
void set(Rect* r, int w, int h);
int area(Rect r);
void show(Rect r);

// 3. 结构体具体定义
typedef struct Rect {
    int width;
    int height;
    void (*set)(Rect*, int, int);  // 注意使用 typedef 后的名称
    int (*area)(Rect);
    void (*show)(Rect);
} Rect;

// 4. 函数实现
void set(Rect* r, int w, int h) {
    r->width = w;
    r->height = h;
}

int area(Rect r) {
    return r.width * r.height;
}

void show(Rect r) {
    printf("Width: %d and height: %d\n", r.width, r.height);
}

// 5. 构造函数（必须在函数定义之后）
void constructRect(Rect* r) {
    r->width = 0;
    r->height = 0;
    r->set = set;
    r->area = area;
    r->show = show;
}

// 6. 主函数
int main() {
    Rect r;
    constructRect(&r);

    r.set(&r, 10, 5);  // 设置宽高
    r.show(r);          // 输出维度
    printf("Rectangle area: %d\n", r.area(r));

    return 0;
}
```

# 15 math.h，time.h
## 15.1 
```
double pow(double x, double y);
double sqrt(double x);
ceil()
floor()

```
记得在编译器中加载指令
```
gcc your_program.c -o your_program -lm
```
## 15.2 time.h
大多数是long类型的，因此使用%ld来存储

获取当前时间
```
time_t now = time(NULL);
printf("Current time is :%s\n",ctime(&now));
```
![[Pasted image 20250329112036.png]]
```C
#include <stdio.h>
#include <time.h>

int main() {
    time_t now = time(NULL);       // 获取当前时间
    struct tm *local = localtime(&now);  // 转换为本地时间

    printf("当前时间：%02d:%02d:%02d\n", local->tm_hour, local->tm_min, local->tm_sec);
    printf("日期：%d-%02d-%02d\n", local->tm_year + 1900, local->tm_mon + 1, local->tm_mday);//注意月份+1，年份+1900
    printf("星期：%d (0=周日, 1=周一...)\n", local->tm_wday);
    printf("今年的第 %d 天\n", local->tm_yday + 1);
    printf("是否夏令时：%d\n", local->tm_isdst);

    return 0;
}

```

另一种方法，使用strftime()
```
int main() {
    time_t now = time(NULL);            // 获取当前时间
    struct tm *local = localtime(&now); // 转换为本地时间

    char buffer[100];  // 定义存储时间字符串的缓冲区

    // 示例 1: 常规日期和时间
    strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", local);
    printf("当前时间：%s\n", buffer);

    // 示例 2: 显示星期几和完整月份名
    strftime(buffer, sizeof(buffer), "今天是 %A, %B %d, %Y", local);
    printf("%s\n", buffer);

    // 示例 3: 显示简写格式
    strftime(buffer, sizeof(buffer), "简写日期: %a, %b %d", local);
    printf("%s\n", buffer);

    return 0;
}
```

`difftime()` 函数 — 计算时间差
```
time_t start_time, end_time;
double differ_seconds;

start_time = time(NULL);

//DO SOMETHINGS

end_time = time(NULL);
differ_seconds = difftime(end_time, start_time);
printf("Duration: %.2f seconds\n",diff_seconds);

return 0;
```
## 15.3 ctype.h
- **`isalpha(c)`**：判断 `c` 是否为字母字符（A-Z 或 a-z）。
    
- **`isdigit(c)`**：判断 `c` 是否为数字字符（0-9）。
    
- **`isalnum(c)`**：判断 `c` 是否为字母或数字字符（A-Z、a-z、0-9）。
    
- **`isspace(c)`**：判断 `c` 是否为空白字符（如空格、制表符`\t`、换行符`\n`等）。
    
- **`islower(c)`**：判断 `c` 是否为小写字母（a-z）。
    
- **`isupper(c)`**：判断 `c` 是否为大写字母（A-Z）。
    
- **`ispunct(c)`**：判断 `c` 是否为标点符号字符（如 `!`, `@`, `#`, `?` 等）。
    
- **`isprint(c)`**：判断 `c` 是否为可打印字符（包括空格但不包括控制字符）
```
#include <stdio.h>
#include <ctype.h> // 引入ctype.h头文件

int main() {
    char ch = 'A';

    if (isalpha(ch)) printf("%c 是字母\n", ch);
    if (isdigit(ch)) printf("%c 是数字\n", ch);
    if (isalnum(ch)) printf("%c 是字母或数字\n", ch);
    if (isspace(ch)) printf("%c 是空白字符\n", ch);
    if (islower(ch)) printf("%c 是小写字母\n", ch);
    if (isupper(ch)) printf("%c 是大写字母\n", ch);
    if (ispunct(ch)) printf("%c 是标点符号\n", ch);
    if (isprint(ch)) printf("%c 是可打印字符\n", ch);

    return 0;
}

```
## 15.4 stdlib.h
### **. `atoi()` — 将字符串转换为整数**

- **原型：** `int atoi(const char *str);`
    
- **功能：** 将字符串 `str` 转换为**整数**（`int` 类型）。
    
- **返回值：**
    
    - 成功时返回转换后的整数值。
        
    - 若 `str` 不是有效的数字字符串，则返回 0。
//比如转化“abc123”
### **`strtod()` — 将字符串转换为浮点数**

- **原型：** `double strtod(const char *str, char **endptr);`
    
- **功能：** 将字符串 `str` 转换为**双精度浮点数**（`double` 类型）。
    
- **参数说明：**
    
    - `str`：要转换的字符串。
        
    - `endptr`：指向字符串中**第一个无效字符**的指针（用于错误检测）。
        
- **返回值：**
    
    - 成功时返回转换后的浮点数。
        
    - 若 `str` 不是有效数字字符串，则返回 0
```
int main() {
    const char *str = "123.45abc";  // 目标字符串
    char *endptr;                   // 存储无效字符的位置

    double num = strtod(str, &endptr);

    printf("转换后的数值：%f\n", num); //转换后的数值：123.450000
    printf("未转换部分的字符串：%s\n", endptr); //未转换部分的字符串：abc

    return 0;
}
```

### rand()和srand(seed)
- `rand()` 生成一个随机整数。
- `srand(seed)` 用于设置随机数生成器的种子（seed）。
```
int main() {
    srand(time(NULL));  // 以当前时间作为种子

    printf("第一次随机数: %d\n", rand());
    printf("第二次随机数: %d\n", rand());
    printf("第三次随机数: %d\n", rand());
    
    return 0;
//第一次随机数: 135792468
第二次随机数: 987654321
第三次随机数: 246813579
}
```
### `abs()和div(numer, denom)`
- `abs()` 返回整数的绝对值。
- `div(numer, denom)` 执行整数除法，并返回一个 `div_t` 结构，该结构包含 **商（quotient）** 和 **余数（remainder）**。
```
typedef struct { 
    int quot;  // 商
    int rem;   // 余数
} div_t;
```

```
int main() {
    int x = -10;
    printf("abs(%d) = %d\n", x, abs(x));  // 取绝对值，abs(-10) = 10

    int a = 17, b = 4;
    div_t result = div(a, b);  // 计算 a / b 的商和余数
    printf("%d / %d = 商: %d, 余数: %d\n", a, b, result.quot, result.rem);
    //17 / 4 = 商: 4, 余数: 1

    return 0;
}
```

# 16 Binary File
## 16.1 `fwrite()`
基本格式：
```
fwrite(*ptr, size, count, *stream);
```
### 参数说明：

- **ptr**：指向要写入的数据的内存地址（可以是数组、结构体等的指针）。
- **size**：每个元素的大小（以字节为单位）。
- **count**：要写入的元素个数。
- **stream**：指向 `FILE` 对象的指针，用于标识输出的文件流。
- 注意在打开文件的时候： =`wb代表writing`， `ab代表appending`， `rb代表reading`

例子：
```
FILE *fp = fopen("data.bin", "wb"); //注意这个大写很重要
int arr[] = {1, 2, 3, 4, 5};
fwrite(arr, sizeof(int), 5, fp);
fread(&data, sizeof(data), 1, fp);
fclose(fp);
```
## 16.2 `fseek()`
```
int fseek(FILE *stream, long offset, int origin);
```

| 参数名    | 含义                      |
| ------ | ----------------------- |
| stream | 文件指针（即 fopen 返回的 FILE*） |
| offset | 偏移量，以字节为单位              |
| origin | 起始位置，指定从哪开始偏移           |
### ✅ `origin` 的取值（三个宏定义）：

|宏常量|值|说明|
|---|---|---|
|`SEEK_SET`|0|从文件**开头**开始偏移|
|`SEEK_CUR`|1|从当前指针位置开始偏移|
|`SEEK_END`|2|从文件**末尾**开始偏移|
例子：
```
FILE *fp = fopen("data.bin", "rb");
fseek(fp, 100, SEEK_SET); // 从文件开头跳过100字节
// 现在可以 fread 或 fwrite 从第101个字节开始操作
fclose(fp);

```

## 16.3 Binary_operation
- **按位与（&）** 用于**清除**特定位。
- **按位或（|）** 用于**设置**特定位。
- **按位异或（^）** 用于**翻转**特定位。
### **左移运算符（<<）**

- **作用：** 将二进制数的所有位向左移动指定的位数，右侧空出的位用零填充。
- **等效于：** 将数字乘以 2 的若干次方。例如，`num << 1` 相当于将 `num` 乘以 `2`，`num << 2` 相当于将 `num` 乘以 `4`，以此类推。
- 
    **示例：**
    `00000011  (3) << 1 => 00000110  (6)`
    
### **右移运算符（>>）**

- **作用：** 将二进制数的所有位向右移动指定的位数。
- **逻辑右移（Logical Shift）** 和 **算术右移（Arithmetic Shift）** 的区别在于如何填充空出来的位。
    
#### **逻辑右移（Logical Shift）**

- **作用：** 将 vacated 位填充为零。
- **适用于：** 无符号整数（unsigned integers）。这种移动方式不考虑数值的符号位，只是纯粹按位操作。
    **示例：**
    
    `00000011  (3) >> 1 => 00000001  (1)  // 逻辑右移 `
    `11111111  (-1) >> 1 => 01111111  (127)  // 逻辑右移`
    

#### **算术右移（Arithmetic Shift）**

- **作用：** 将 vacated 位填充为符号位（即最左边的位）。
    
- **适用于：** 有符号整数（signed integers）。这种移动方式考虑到数值的符号，确保数值的符号不改变。
    
    - 如果符号位为 `0`（表示正数），则填充为 `0`。
        
    - 如果符号位为 `1`（表示负数），则填充为 `1`。
        
    
    **示例：**
    
    cpp
    
    复制编辑
    
    `00000011  (3) >> 1 => 00000001  (1)  // 算术右移，正数保持符号 11111111  (-1) >> 1 => 11111111  (-1)  // 算术右移，负数保持符号`
    

### **总结：**

- **左移（<<）**：将位向左移，右侧填充零，相当于乘以 2 的若干次方。
    
- **右移（>>）**：
    
    - **逻辑右移**：将位向右移，左侧填充零，常用于无符号整数。
        
    - **算术右移**：将位向右移，左侧填充符号位，常用于有符号整数，确保负数保持为负数，正数保持为正数。

# 17 一些函数

## 17.1 qsort()

用来排序 [C语言qsort函数的用法！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1QRk7Y7ER3/?spm_id_from=333.337.search-card.all.click&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61)
```C
#include<stdio.h>
#include<stdlib.h>

int cmp(const void *x, const void *y){
        return *(int*)x - *(int*)y;
    }  //这样是从小到大排序

int main(){
    
    int a[] = {8,2,4,5,6,7,9,1,3};
    int lenth = sizeof(a)/sizeof(int);
    qsort(a,lenth,sizeof(int),cmp);
    for(int i =0; i<lenth;i++){
        printf("%d",a[i]);
    }
    return 0;
}
```

# 18 考前笔记
1. 定义一个浮点数，float a = 3.14159F; 注意结尾的F
2. sprintf的使用
3. Ex_ch 3 的第三题，注意strlen以及sizeof 的区别，同时注意把ACSII转化为数值的办法。`sum += (nums[i]-'0')*(nums[i]-'0');`
4. 如何检测输入的值是否为想要的类型？比如输入一串数 字，输入字母的时候停下？参考Ex_ch 5 的第一题，
```C
int input[10];
    printf("Enter the elements:\n");

    int i =0;
    while(i<10 && scanf("%d",&input[i])==1){ //这里
        i++;
    }
```
5. 验证是否为偶数，使用%取余，只有char类型的数组可以不需要  Ex_ch5 2
6. Ex_ch5 4指针练习，5动态分配问题[[动态分配存储字符串容器]]
7. 注意看getline，读取不到返回-1。 注意free的时机，必须在最后释放，和malloc区分开
8. Ex_Ch6 第三题 打开模式fopen哪里是“r”而不是‘r’。`token[strcspn(token,"\r\n")] = '\0';`本质token需要指向有效的内存区域。

#### **题目1：字符串处理（3种可能）**

- **变体1：统计字符串中字符和元音数量**
```C
#include<stdio.h>
#include<string.h>

int main(){
    char str[100];
    printf("Enter a sentence:\n");
    fgets(str,sizeof(str)/sizeof(char),stdin);

    str[strcspn(str,"\n")] = '\0';

    int nums = strlen(str);
    int counts;
    for(int i=0;i<nums;i++){
        if(strchr("aeiouAEIOU",str[i])!=NULL){
            counts++;
        }
    }
    printf("Character nums: %d, vowels number %d\n", nums,counts);
    
    return 0;
    
}
```
- **变体2：使用 `strcmp()` 比较字符串**
```
int main(){
    char password[] = "abcdef";
    char input[10];
    printf("Input the password!\n");
    scanf("%s",input);
    int result = strcmp(password,input);
    switch (result){
        case 0:
        printf("Password correct!\n");
        break;

        default:
        printf("Incorrect!\n");
        break;
    }
       
}
```
- **变体3：未知，但可能相关字符串函数（如 `strlen`、`strcpy`）**
```C
int main(){
    char *line = NULL;
    size_t len = 0;
    __ssize_t read;

    printf("Input the thing:\n");
    while(read = getline(&line,&len,stdin)!=-1){
        printf("You entered: %s",line);
    }

    free(line);
}

---
int main(){
    while (1){
        char str[51];
        printf("Input the name please.\n");
        int num = strlen(str);
        fgets(str,sizeof(str),stdin);
        str[strcspn(str,"\n")] ='\0';
        if(*str == '0'){
            break;
        }else{
            printf("Hello %s.\n",str);
        }
    }
    return 0;
    
}
```


📌**考点：字符串遍历、条件判断、字符比较**

---

#### **题目2：数学计算函数（3种可能）**

- **变体1：计算圆的面积（给定半径）**
```C
#define PI 3.14
int main(){
    printf("Input the banjing:\n");
    float r ;
    scanf("%f",&r);
    float result = PI*r*r;
    printf("The area is %.2f.\n",result);
    return 0;
}
```
- **变体2：摄氏转华氏（Celsius to Fahrenheit）**
```C
float convert(float celsius){
    float result  = (celsius*9.0/5.0) +32;
    return result;
}
int main(){
    float a;
    printf("Enter number:\n");
    scanf("%f",&a);
    float result = convert(a);
    printf("%.2f C is equal to %f F.\n",a,result);
}
```
- **变体3：数组最大值

📌**考点：函数定义与调用、数学表达式、基本输入输出**

---

#### **题目3：数组操作（3种可能）**

- **变体1：数组中所有元素求和**
    
- **变体2：固定大小数组中最大值**
```C
//甚至可以使用ctrl+d来进行终止
int max(int nums[],int count){
    int large = 0;
    for(int i=0;i<count;i++){
        if(nums[i]>large) large = nums[i];
    }
    return large;
}
int main(){
    printf("Input numbers:\n");
    int size = 10;
    int *nums = malloc(size*sizeof(int));
    if(nums ==NULL){
        printf("Memory allocation failed.\n");
    }
    int i = 0;
    int temp;
    while (scanf(" %d",&temp) ==1)//这里重要，成功是1，不成功是0，EOF是-1
    {
        if(i >size/2){
            size = size*2;
            nums = realloc(nums,size*sizeof(int));
        } 
        nums[i] = temp;
        i++;
    }
    
    int result = max(nums,i);
    printf("Result is %d\n",result);
    free(nums);
    
}
```
- **变体3：未知，可能是数组反转、平均值等**
    

📌**考点：数组遍历、条件判断、循环**

---

#### **题目4：链表操作（2种可能）**

- **变体1：在链表末尾插入节点（InsertLast）**
    
- **变体2：链表头部插入节点 
    
```C
#include<stdio.h>
#include<stdlib.h>

typedef struct node{
    int value;
    struct node* next;
}Node;

typedef struct{
    Node *head;
}List;

List *create(){
    List* list = malloc(sizeof(List));
    list->head = NULL;
    return list;
}

void insert(List *list, int value){
    Node *n = malloc(sizeof(Node));
    n->value = value;

    if(list->head ==NULL){
        list->head = n;
    }else{
        Node *cur = list->head;
        while (cur->next !=NULL)
        {
            cur = cur->next;
        }
        cur->next = n; 
    }
}

void insert_head(List *list, int value){
    Node *n = malloc(sizeof(Node));
    n->value = value;
    n->next = list->head;
    list->head = n;
}

void print(Node *node){
    int i =0;
    while (node!=NULL)
    {
        printf("Node %d value %d\n",i+1,node->value);
        i++;
        node = node->next;
    }
    
}
int main(){
    List *list = create();
    printf("Input the numbers:\n");
    while (1)
    {
        int value;
        scanf(" %d",&value);
        if(value==0){
            break;
        }else{
            insert_head(list,value);
        }
        
    }
    print(list->head);
    
    return 0;
}
```
📌**考点：指针、结构体、链表操作**

---

#### **题目5：位运算（2种可能）**

- **变体1：判断无符号整数中第n位是否为1**
    
    - 示例代码：`return u & (1 << uPos);`
```C
int IsBitSet(unsigned int m, unsigned int n){
    return (m>>n)&1;
}

int main(){
    printf("Input the unsigned number and unsigned place:\n");
    int m,n;
    scanf("%u %u",&m,&n);
    int value = IsBitSet(m,n);
    printf("%d\n",value);
}
```
        
- **变体2：统计无符号整数中所有的1的个数（用位运算）**
```C
int lowBit(int n){
    return n&(-n);
}
int countUpBits(unsigned int num){
    int count = 0;
    while (num)
    {
        num = num-lowBit(num);
        count++;
    }
    return count;
}

int main(){
    printf("Enter an unsigned integer:\n");
    unsigned int m;
    scanf(" %d",&m);
    int num = countUpBits(m);
    printf("Amount of 1 : %d\n ",num);
}
```

📌**考点：位运算（AND、移位）、逻辑判断**


# C++
注意这两个头文件
```
#include<iostream>
using namespace std;
```

布尔类型：非零为true，0为false
```
#include<iostream>
using namespace std;

int main(){
    bool s = true;
    bool m  = 1;
    cout << s <<" "<< m <<" "<< endl;  //默认都是打印数字的
    //如果想要打印布尔类型的
    cout <<boolalpha << s <<endl;
    cout <<noboolalpha << m << endl;
}
```

## 1. 输入输出
当想要输入一个句子的时候，使用getline
```
string fullName;
cout << "请输入你的全名：";
getline(cin, fullName); // 使用 getline 读取整行输入
```

平方矩阵[753. 平方矩阵 I - AcWing题库](https://www.acwing.com/problem/content/755/)
这个题目的思路在于打印出来的结果即该位置到上下左右的最小距离
```cpp
#include<iostream>
using namespace std;
int main(){
    int n;
    while(cin >> n,n){  //首先执行输入n，如果不为0则进入循环
        for(int i = 1;i<=n;i++){ //行
            for(int j = 1;j<=n;j++){  //列
                    int l = j, r = n-j + 1, up = i, down = n - i + 1;
                    cout << min(min(l,r),min(up,down)) << ' ';
            }
            cout << endl;
        }
        cout << endl;
    }
}
```

## 2. 常用库函数
reverse()，这里的区间为`[begin,end)` 然后进行反转
```
int main(){
    int nums[20];
    for(int i = 0;i<20;i++){
        cin >> nums[i];
    }

    reverse(nums,nums+20); //左闭右开进行反转

    for(int i = 0;i<20;i++){
        printf("N[%d] = %d\n",i,nums[i]);
    }
}
```
min_element
```
int main(){
    int n;
    cin >> n;
    int nums[n];
    for(int i = 0;i<n;i++){
        cin >> nums[i];
    }
    auto it = min_element(nums,nums+n); //这里的auto可以自动推断类型，it实际是指针
    cout << "Minimum value: " << *it << endl;
    cout << "Position: " << it-nums << endl;
}
```
