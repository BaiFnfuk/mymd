## 寄存器操作

#### 把变量的某位清零

```c
int a = 0x9f;
// a的二进制为 1001 1111
// 对a的第二位清零
a &= ~(1 << 2);
// 1左移两位： 0000 0100
// 取反： 1111 1011
/* 位与运算：
	1001 1111
   &1111 1011
	1001 1011
*/
```

- 对变量的某位清零的公式

**a &= ~(1<<n)**, n为要清零的位置

#### 对连续的几位清零

```c
int a = 0x9f;
// a的二进制为 1001 1111
// 对a的第二位清零
// 把a的二进制位分成两个或四个一组，两个一组可分成四组，四个一组可分成两组
// 对a的2,3位清零
a &= ~(0x3 <<2*1);
// 0x3 = 0000 0011
// 0x3左移2位 0000 1100
// 取反 1111 0011
/* 位运算
	1001 1111
   &1111 0011
    1001 0011
*/
```

- 对变量的某几个连续位清零公式

**a &= ~(m << n*p)** m是组内所有位全部为1时的值，n是每组有多少位，p是第几组，从0开始算起

#### 对变量的某几位进行赋值

```c
int a = 0x9f;
// 把a的二进制位分成两个一组，对第2组进行赋值
a &= ~(0x3 <<2*1); //先清零
a |= (1<<2*2)	// 赋值
```

- 对变量的某几位进行赋值公式

**a |= (m << n*p)** m是要赋的值，n是每组有多少位，p是第几组，从0开始算起

#### 对变量的某位取反

```c
int a = 0x9f;
//把 bit6 取反，其它位不变
a ^= (1<<6);
```

