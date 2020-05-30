## 面试题

###  用预处理指令#define 声明一个常数，用以表明1年中有多少秒（忽略闰年问题）

- ```c
  #define SECONDS_PER_YEAR	(60*60*24*365)UL
  ```

- \#define 语法的基本知识

-  意识到这个表达式将使一个16位机的整型数溢出-因此要用到长整型符号L,告诉编译器这个常数是的长整型数。

  

### 写一个"标准"宏MIN ，这个宏输入两个参数并返回较小的一个。

- ```c
  #define MIN(A,B) ((A)<=(B)?(A):(B))
  ```

- 标识#define在宏中应用的基本知识

-  三重条件操作符的知识。

- 懂得在宏中小心地把参数用括号括起来



### 嵌入式系统中经常要用到无限循环，你怎么样用C编写死循环呢？

- ```c
  while(1)
  {
      //TODO
  }
  ```



###  用变量a给出下面的定义

- a) 一个整型数（An integer）

- b) 一个指向整型数的指针（ A pointer to an integer）

- c) 一个指向指针的的指针，它指向的指针是指向一个整型数（ A pointer to a pointer to an intege）r

- d) 一个有10个整型数的数组（ An array of 10 integers）

- e) 一个有10个指针的数组，该指针是指向一个整型数的。（An array of 10 pointers to integers）

- f) 一个指向有10个整型数数组的指针（ A pointer to an array of 10 integers）

- g) 一个指向函数的指针，该函数有一个整型参数并返回一个整型数（A pointer to a function that takes an 

  integer as an argument and returns an integer）

- h) 一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数（ An array of ten 

  pointers to functions that take an integer argument and return an integer ）

```c
a) int a;
b) int *a;
c) int **a;
d) int a[10];
e) int *a[10];
f) int (*a)[10];
g) int (*a)(int);
h) int (*a[10])(int);
```



### 关键字static的作用是什么？

- 在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。
-  在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所用函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。
- 在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。



### 关键字const有什么含意？

- const意味着"只读"就可以了

- const int a;
- int const a;
- const int *a;
- int const *a;
- int *const  a ;

const int a 与 int const a是一样的，a是一个常整型数。

int *const  a; a值不能改变。

const int *a; int const *a;指针所指向的数据是只读的，也就是 a 本身的值可以修改（指向不同的数据），但它们指向的数据不能被修改。

