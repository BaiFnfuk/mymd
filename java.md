## java

### 标识符

- 标识符由数字（0~9）和字母（A~Z 和 a~z）、美元符号（$）、下划线（_）以及 Unicode 字符集中符号大于 0xC0 的所有符号组合构成（各符号之间没有空格）。

- 标识符的第一个符号为字母、下划线和美元符号，后面可以是任何字母、数字、美元符号或下划线。

### 关键字

​		Java 语言目前定义了 51 个关键字，这些关键字不能作为变量名、类名和方法名来使用。以下对这些关键字进行了分类。

1. 数据类型：boolean、int、long、short、byte、float、double、char、class、interface。
2. 流程控制：if、else、do、while、for、switch、case、default、break、continue、return、try、catch、finally。
3. 修饰符：public、protected、private、final、void、static、strict、abstract、transient、synchronized、volatile、native。
4. 动作：package、import、throw、throws、extends、implements、this、supper、instanceof、new。
5. 保留字：true、false、null、goto、const。

### 注释

1. 单行注释：

   ```java
   // 这是一行注释
   ```

2. 多行注释：

   ```java
   /*
    这是多行注释
    这是多行注释
     这是多行注释
   */
   ```

3. 文档注释：

   ```java
   /**
   *  文档注释
   */
   ```

### 常量

#### 常量类型

1. 整型常量

   - 十进制数形式：如 54、-67、0。

   - 八进制数形式：Java 中的八进制常数的表示以 0 开头，如 0125 表示十进制数 85，-013 表示十进制数 -11。

   - 十六进制数形式：Java 中的十六进制常数的表示以 0x 或 0X 开头，如 0x100 表示十进制数 256，-0x16 表示十进制数 -22。
   
   **整型（int）常量默认在内存中占 32 位（4字节），是具有整数类型的值，当运算过程中所需值超过 32 位长度时，可以把它表示为长整型（long）数值。长整型类型则要在数字后面加 L 或 1， 如 697L，表示一个长整型数，它在内存中占 64 位（8字节）。**
   
2. 实型常量

   - 十进制数形式：由数字和小数点组成，且必须有小数点，如 12.34、-98.0。
   - 科学记数法形式：如 1.75e5 或 32&E3，其中 e 或 E 之前必须有数字，且 e 或 E 之后的数字必须为整数。

   **Java 实型常量默认在内存中占 64 位（8字节），是具有双精度型（double）的值。如果考虑到需要节省运行时的系统资源，而运算时的数据值取值范围并不大且运算精度要求不太高的情况，可以把它表示为单精度型（float）的数值。单精度型数值一般要在该常数后面加 F 或 f，如 69.7f，表示一个 float 型实数，它在内存中占 32 位（4字节）（取决于系统的版本高低）。**

3. 布尔型常量

   - Java 的布尔型常量只有两个值，即 false（假）和 true（真）。

4. 字符型和字符串常量

   - Java 的字符型常量值是用单引号引起来的一个字符，如 'e'、‘E'。需要注意的是，Java 字符串常量值中的单引号和双引号不可混用。双引号用来表示字符串，像 "11"、"d" 等都是表示单个字符的字符串。

#### 常量定义

**Java 语言使用 final 关键字来定义一个常量，其语法如下所示：**

```java
final dataType variableName = value;
//final 是定义常量的关键字，dataType 指明常量的数据类型，variableName 是变量的名称，value 是初始值。

public class HelloWorld{
    // 静态常量
    public static final double PI = 3.14;
    // 声明成员变量
    final int y = 10;
    
    public static void main(String[] args){
        // 声明局部常量
        final double x = 3.3;
    }
}
```

1. 静态常量

```java 
public static final double PI = 3.14;
// 不需要创建类实例，直接用类名就能访问
HelloWorld.PI
```

2. 成员常量

```java
final int y = 10;
// 作用域类似于成员变量，但不能修改。
```

3. 局部常量

```java
final double x = 3.3;
// 作用域类似于局部变量，但不能修改。
```

在定义常量时，需要注意如下内容：

- 在定义常量时就需要对该常量进行初始化。
- final 关键字不仅可以用来修饰基本数据类型的常量，还可以用来修饰对象的引用或者方法。
- 为了与变量区别，常量取名一般都用大写字符。

### 变量

**在 java 中用户可以通过指定数据类型和标识符来声明变量，其基本语法如下所示：**

```java
DataType identifier;
DataType identifier=value;

String employee;    // String 类型的变量
boolean isSave;    // boolean 类型的变量
int createAt = 10;    // int 类型的变量
```

Java 中初始化变量时需要注意以下事项：

- 变量是类或者结构中的字段，如果没有显式地初始化，默认状态下创建变量并默认初始值为 0。
- 方法中的变量必须显式地初始化，否则在使用该变量时就会出错。

### 变量作用域

1. 成员变量

   Java的成员变量有两种，分别是全局变量和静态变量（类变量）。定义在方法体和语句块之外，不属于任何一个方法，作用域是整个类。

   |         名称         |     修饰     |            访问            |                           生命周期                           |
   | :------------------: | :----------: | :------------------------: | :----------------------------------------------------------: |
   | 全局变量（实例变量） | 无static修饰 |       对象名.变量名        |             只要对象被当作引用，实例变量就将存在             |
   |  静态变量（类变量）  | 用static修饰 | 类名.变量名或对象名.变量名 | 其生命周期取决于类的生命周期。类被垃圾回收机制彻底回收时才会被销毁 |

   

2. 局部变量

   局部变量是指在方法或者方法代码块中定义的变量，其作用域是其所在的代码块。可分为以下三种：

   - 方法参数变量（形参）：在整个方法内有效。
   - 方法局部变量（方法内定义）： 从定义这个变量开始到方法结束这一段代码内有效。
   - 代码块局部变量（代码块内定义）：从定义这个变量开始到代码块结束这一段时间内有效。

   **局部变量在使用前必须被程序员主动初始化值。**

### 数据类型

Java 语言支持的数据类型分为两种：基本数据类型（Primitive Type）和引用数据类型（Reference Type）。

#### 基本数据类型

基本数据类型包括 boolean（布尔型）、float（单精度浮点型）、char（字符型）、byte（字节型）、short（短整型）、int（整型）、long（长整型）和 double （双精度浮点型）共 8 种

|   类型名称   | 关键字  | 占用内存 |                  取值范围                  |
| :----------: | :-----: | :------: | :----------------------------------------: |
|    字节型    |  byte   |  1字节   |                  -128~127                  |
|    短整型    |  short  |  2字节   |                -32768~32767                |
|     整型     |   int   |  4字节   |           -2147483648~2147483647           |
|    长整型    |  long   |  8字节   | -9223372036854775808L~9223372036854775807L |
| 单精度浮点型 |  float  |  4字节   |        +/-3.4E+38F（6~7 个有效位）         |
| 双精度浮点型 | double  |  8字节   |         +/-1.8E+308 (15 个有效位）         |
|    字符型    |  char   |  2字节   |               ISO 单一字符集               |
|    布尔型    | boolean |  1字节   |               true 或 false                |

所有的基本数据类型的大小（所占用的字节数）都已明确规定，在各种不同的平台上保持不变，这一特性有助于提高 Java 程序的可移植性。

#### 引用数据类型

引用数据类型建立在基本数据类型的基础上，包括数组、类和接口。引用数据类型是由用户自定义，用来限制其他数据的类型。

### 数据类型转换

#### 隐式转换

如果以下 2 个条件都满足，那么将一种类型的数据赋给另外一种类型变量的时，将执行自动类型转换（automatic type conversion）。

- 两种数据类型彼此兼容
- 目标类型的取值范围大于源数据类型（低级类型数据转换成高级类型数据）

自动转换的规则是从低级类型数据转换成高级类型数据。转换规则如下：

- 数值型数据的转换：byte→short→int→long→float→double。
- 字符型转换为整型：char→int。

```java
byte b = 50;
b = b * 2;    // Type mismatch: cannot convert from int to byte

byte b = 50;
b = (byte)(b*2);
```

**注意：char 类型比较特殊，char 自动转换成 int、long、float 和 double，但 byte 和 short 不能自动转换为 char，而且 char 也不能自动转换为 byte 或 short。**

#### 显式转换（强制类型转换）

当两种数据类型不兼容，或目标类型的取值范围小于源类型时，自动转换将无法进行，这时就需要进行强制类型转换

```java
int a = 3;
double b = 5.0;
a = (int)b;
```

在强制类型转换中，如果是将浮点类型的值转换为整数，直接去掉小数点后边的所有数字；而如果是整数类型强制转换为浮点类型时，将在小数点后面补零。

### 算术运算符

#### 一元运算符

| 运算符 |   名称   |             说明             |    例子    |
| :----: | :------: | :--------------------------: | :--------: |
|   -    | 取反符号 |           取反运算           |    b=-a    |
|   ++   |  自加一  | 先取值再加一，或先加一再取值 | a++ 或 ++a |
|   --   |  自减一  | 先取值再减一，或先减一再取值 | a-- 或 --a |

#### 二元运算符

| 运 算 符 | 名 称 |                          说 明                           | 例 子 |
| :------: | :---: | :------------------------------------------------------: | :---: |
|    +     |  加   | 求 a 加 b 的和，还可用于 String 类型，进行字符串连接操作 | a + b |
|    -     |  减   |                      求 a 减 b 的差                      | a - b |
|    *     |  乘   |                     求 a 乘以 b 的积                     | a * b |
|    /     |  除   |                     求 a 除以 b 的商                     | a / b |
|    %     | 取余  |                    求 a 除以 b 的余数                    | a % b |

**算术运算符都是双目运算符，即连接两个操作数的运算符。优先级上，*、/、％ 具有相同运算级别，并高于 +、-（+、- 具有相同级别）。**

进行算术运算时应注意以下两点：

- 求余（％）运算要求参与运算的两个操作数均为整型，不能为其他类型。
- 两个整数进行除法运算，其结果仍为整数。如果整数与实数进行除法运算，则结果为实数。

#### 算术赋值运算符

| 运 算 符 |  名 称   |      例 子       |
| :------: | :------: | :--------------: |
|    +=    |  加赋值  | a += b、a += b+3 |
|    -=    |  减赋值  |      a -= b      |
|    *=    |  乘赋值  |      a *= b      |
|    /=    |  除赋值  |      a /= b      |
|    %=    | 取余赋值 |      a %= b      |

### 逻辑运算符

逻辑运算符把各个运算的关系表达式连接起来组成一个复杂的逻辑表达式，以判断程序中的表达式是否成立，判断的结果是 true 或 false。

| 运算符 |  用法  |  含义  |                        说明                        |    实例    | 结果  |
| :----: | :----: | :----: | :------------------------------------------------: | :--------: | :---: |
|   &&   |  a&&b  | 短路与 |  ab 全为 true 时，计算结果为 true，否则为 false。  |  2>1&&3<4  | true  |
|  \|\|  | a\|\|b | 短路或 | ab 全为 false 时，计算结果为 false，否则为 true。  | 2<1\|\|3>4 | false |
|   !    |   !a   | 逻辑非 | a 为 true 时，值为 false，a 为 false 时，值为 true |   !(2>4)   | true  |
|   \|   |  a\|b  | 逻辑或 |  ab 全为 false 时，计算结果为 false，否则为 true   |  1>2\|3>5  | false |
|   &    |  a&b   | 逻辑与 |   ab 全为 true 时，计算结果为 true，否则为 false   |  1<2&3<5   | true  |

- && 与 & 区别：如果 a 为 false，则不计算 b（因为不论 b 为何值，结果都为 false）
- || 与 | 区别：如果 a 为 true，则不计算 b（因为不论 b 为何值，结果都为 true）

**短路与（&&）和短路或（||）能够采用最优化的计算方式，从而提高效率。在实际编程时，应该优先考虑使用短路与和短路或。**

### 关系运算符

关系运算符是二元运算符，运算结果是 boolean 型。当运算符对应的关系成立时，运算结果是 true，否则是 false。

| 运算符 |       含义       |                             说明                             |              实例               |         结果         |
| :----: | :--------------: | :----------------------------------------------------------: | :-----------------------------: | :------------------: |
|   >    |    大于运算符    | 只支持左右两边操作数是数值类型。如果前面变量的值大于后面变量的值， 则返回 true。 |               2>3               |        false         |
|   >=   | 大于或等于运算符 | 只支持左右两边操作数是数值类型。如果前面变量的值大于等于后面变量的值， 则返回 true。 |              4>=2               |         true         |
|   <    |    小于运算符    | 只支持左右两边操作数是数值类型。如果前面变量的值小于后面变量的值，则返回 true。 |               2<3               |         true         |
|   <=   | 小于或等于运算符 | 只支持左右两边操作数是数值类型。如果前面变量的值小于等于后面变量的值， 则返回 true。 |              4<=2               |        false         |
|   ==   |    相等运算符    | 如果进行比较的两个操作数都是数值类型，无论它们的数据类型是否相同，只要它们的值相等，也都将返回 true。 如果两个操作数都是引用类型，只有当两个引用变量的类型具有父子关系时才可以比较，只要两个引用指向的不是同一个对象就会返回 true。 Java也支持两个 boolean 类型的值进行比较。 | 4==4 97=='a' 5.0==5 true==false | true true true false |
|   !=   |   不相等运算符   | 如果进行比较的两个操作数都是数值类型，无论它们的数据类型是否相同，只要它们的值不相等，也都将返回 true。 如果两个操作数都是引用类型，只有当两个引用变量的类型具有父子关系时才可以比较，只要两个引用指向的不是同一个对象就会返回 true。 |              4!=2               |         true         |

### 位运算符

#### 位逻辑运算符

位逻辑运算符包含 4 个：&（与）、|（或）、~（非）和 ^（异或）。除了 ~（即位取反）为单目运算符外，其余都为双目运算符。

| 运算符 |          含义           |  实例  | 结果 |
| :----: | :---------------------: | :----: | :--: |
|   &    |  按位进行与运算（AND）  | 4 & 5  |  4   |
|   \|   |  按位进行或运算（OR）   | 4 \| 5 |  5   |
|   ^    | 按位进行异或运算（XOR） | 4 ^ 5  |  1   |
|   ~    | 按位进行取反运算（NOT） |  ~ 4   |  -5  |

##### 位与运算符

位与运算符为`&，其运算规则是：参与运算的数字，低位对齐，高位不足的补零，如果对应的二进制位同时为 1，那么计算结果才为 1，否则为 0。因此，任何数与 0 进行按位与运算，其结果都为 0。

```java
 0000 0000 1100 1000
&0000 0000 0000 0000
 0000 0000 0000 0000
```

##### 位与运算符

位或运算符为`|，其运算规则是：参与运算的数字，低位对齐，高位不足的补零。如果对应的二进制位只要有一个为 1，那么结果就为 1；如果对应的二进制位都为 0，结果才为 0。

```java
 0000 0000 0000 1011
|0000 0000 0000 0111
 0000 0000 0000 1111
```

##### 位异或运算符

位异或运算符为`^，其运算规则是：参与运算的数字，低位对齐，高位不足的补零，如果对应的二进制位相同（同时为 0 或同时为 1）时，结果为 0；如果对应的二进制位不相同，结果则为 1。

```java
 0000 0000 0000 1011
^0000 0000 0000 0111
 0000 0000 0000 1100
```

##### 位取反运算符

位取反运算符为`~，其运算规则是：只对一个操作数进行运算，将操作数二进制中的 1 改为 0，0 改为 1。

```java
~0000 0000 0000 1010
 1111 1111 1111 0101
```

#### 位移运算

| 运算符 |     含义     | 实例 | 结果 |
| :----: | :----------: | :--: | :--: |
|   »    | 右移位运算符 | 8»1  |  4   |
|   «    | 左移位运算符 | 9«2  |  36  |

##### 左位移运算符

左移位运算符为`«，其运算规则是：按二进制形式把所有的数字向左移动对应的位数，高位移出（舍弃），低位的空位补零。

```java
  0000 0000 0000 1011
<<0000 0000 0001 0100     
```

##### 右位移运算符

右位移运算符为`»，其运算规则是：按二进制形式把所有的数字向右移动对应的位数，低位移出（舍弃），高位的空位补零。

```java
  0000 0000 0000 1011
>>0000 0000 0000 0101
```

#### 复合位赋值运算符

| 运算符 |     含义     |     实例      |            结果            |
| :----: | :----------: | :-----------: | :------------------------: |
|   &=   |  按位与赋值  | num1 &= num2  | 等价于 num 1=num 1 & num2  |
|  \|=   |  按位或赋值  | num1 \|= num2 | 等价于 num 1=num 1 \| num2 |
|   ^=   | 按位异或赋值 | num1 ^= num2  | 等价于 num 1=num 1 ^ num2  |
|   -=   | 按位取反赋值 | num1 ~= num2  | 等价于 num 1=num 1 ~ num2  |
|   «=   | 按位左移赋值 | num1 «= num2  | 等价于 num 1=num 1 « num2  |
|   »=   | 按位右移赋值 | num1 »= num2  | 等价于 num 1=num 1 » num2  |

### 三目运算符

条件运算符的符号表示为“?:”，使用该运算符时需要有三个操作数，因此称其为三目运算符。

```java
result = <expression> ? <statement1> : <statement3>;
// expression 是一个布尔表达式。当 expression 为真时，执行 statement1， 否则就执行 statement3。此三元运算符要求返回一个结果，因此要实现简单的二分支程序，即可使用该条件运算符。

int x, y;
x = 6, y = 2;
z = x > y ? x-y : x+y;
```

### 运算符优先级

|          优先级           |          运算符          |  结合性  |
| :-----------------------: | :----------------------: | :------: |
|             1             |        ()、[]、{}        | 从左向右 |
|             2             |    !、+、-、~、++、--    | 从右向左 |
|             3             |         *、/、%          | 从左向右 |
|             4             |           +、-           | 从左向右 |
|             5             |        «、»、>>>         | 从左向右 |
|             6             | <、<=、>、>=、instanceof | 从左向右 |
|             7             |          ==、!=          | 从左向右 |
|             8             |            &             | 从左向右 |
|             9             |            ^             | 从左向右 |
|            10             |            \|            | 从左向右 |
|            11             |            &&            | 从左向右 |
|            12             |           \|\|           | 从左向右 |
|            13             |            ?:            | 从右向左 |
| 14=、+=、-=、*=、/=、&=、 | =、^=、~=、«=、»=、>>>=  | 从右向左 |

### if else

```java
if (条件表达式){
    语句块;
}

if (条件表达式){
    语句块;
}else{
    语句块;
}

if (条件表达式){
    语句块;
}else if (条件表达式){
    语句块;
}else{
    语句块;
}

if (条件表达式){
    if (条件表达式){
    	语句块;
	}else{
      	语句块; 
    }
}else{
    if (条件表达式){
    	语句块;
	}else{
      	语句块; 
    }
}
```

### switch case

```java
switch(表达式) {
    case 值1:
        语句块1;
        break;
    case 值2:
        语句块2;
        break;
    …
    case 值n:
        语句块n;
        break;
    default:
        语句块n+1;
    break;
}

// switch表达式必须为 byte，short，int，char类型。
// Java7 增强了 switch 语句的功能，允许 switch 语句的控制表达式是 java.lang.String 类型的变量或表达式。只能是 java.lang.String 类型，不能是 StringBuffer 或 StringBuilder 这两种字符串的类型。
```

### if语句和switch语句的区别

1. 从使用效率上区分

从使用效率上区分，在对同一个变量的不同值作条件判断时，既可以使用 switch 语句，也可以使用 if 语句。使用 switch 语句的效率更高一些，尤其是判断的分支越多，越明显。

2. 从实用性上区分

从语句的实用性角度区分，switch 语句不如 if 条件语句，if 语句是应用最广泛和最实用的语句。

3. 何时使用 if 语句和 switch 语句

在程序开发的过程中，何时使用 if 语句和 switch 语句，需要根据实际情况而定，应尽量做到物尽其用。不能因为 switch 语句的效率高就一直使用，也不能因为 if 语句常用就不用 switch 语句。需要根据实际情况，具体问题具体分析，使用最适合的条件语句。

### while

```java
while(条件表达式) {
    语句块;
}

do {
    语句块;
}while(条件表达式);
// do-while 循环语句的特点是先执行循环体，然后判断循环条件是否成立。
```

while 循环和 do-while 循环的不同处如下：

- 语法不同：与 while 循环相比，do-while 循环将 while 关键字和循环条件放在后面，而且前面多了 do 关键字，后面多了一个分号。
- 执行次序不同：while 循环先判断，再执行。do-while 循环先执行，再判断。
- 一开始循环条件就不满足的情况下，while 循环一次都不会执行，do-while 循环则不管什么情况下都至少执行一次。

### for

```java
for(条件表达式1;条件表达式2;条件表达式3) {
    语句块;
}
```

### for、do-while 和 while 的区别

|   名称   |                概念                |          适用场景          |                             特点                             |
| :------: | :--------------------------------: | :------------------------: | :----------------------------------------------------------: |
|   for    |  根据循环次数限制做多少次重复操作  |  适合循环次数是已知的操作  | 初始化的条件可以使用局部变量和外部变量。使用局部变量时，控制执行在 for 结束后会自动释放，提高内存使用效率。且变量在 for 循环结束后，不能被访问。先判断，再执行 |
|  while   | 当满足什么条件的时候，才做某种操作 |  适合循环次数是未知的操作  | 初始化的条件只能使用外部变量，且变量在 while 循环结束后可以访问先判断，再执行 |
| do-while |   先执行一次，在判断是否满足条件   | 适合至少执行一次的循环操作 |  在先需要执行一次的情况下，代码更加简洁。先执行一次，再判断  |

### foreach语句

foreach 循环语句是 for 语句的特殊简化版本，主要用于执行遍历功能的循环。

```java
for(类型 变量名:集合) {
    语句块;
}

// 声明并初始化int数组
int[] numbers = { 43, 32, 53, 54, 75, 7, 10 };
System.out.println("----for each----");
// for-each语句
for (int item : numbers) {
    System.out.println("Count is:" + item);
}
```

Java 还提供了 break、continue 和 return 来控制程序的循环结构，作用如下：

- break：表示跳出当前层循环
- continue：表示跳出本次循环，进入下一次循环
- return：跳出当前方法的循环

### 字符串

在 Java 中定义一个字符串最简单的方法是用双引号把它包围起来。这种用双引号括起来的一串字符实际上都是 String 对象，如字符串“Hello”在编译后即成为 String 对象。因此也可以通过创建 String 类的实例来定义字符串。

**不论使用哪种形式创建字符串，字符串对象一旦被创建，其值是不能改变的，但可以使用其他变量重新赋值的方式进行更改。**

```java
// 直接定义字符串
String str = "Hello World!";
```

字符串变量必须经过初始化才能使用。

#### String构造函数

1. String()

   初始化一个新创建的 String 对象，表示一个空字符序列。

2. String(String original)

   初始化一个新创建的 String 对象，使其表示一个与参数相同的字符序列。换句话说，新创建的字符串是该参数字符串的副本。

   ```java
   String str1 = new String("Hello Java");
   String str2 = new String(str1);
   ```

3. String(char[] value)

   分配一个新的字符串，将参数中的字符数组元素全部变为字符串。该字符数组的内容已被复制，后续对字符数组的修改不会影响新创建的字符串。

   ```java
   char a[] = {'H','e','l','l','0'};
   String sChar = new String(a);
   a[1] = 's';
   ```

4. String(char[] value, int offest, int count)

   分配一个新的 String，它包含来自该字符数组参数一个子数组的字符。offset 参数是子数组第一个字符的索引，count 参数指定子数组的长度。该子数组的内容已被赋值，后续对字符数组的修改不会影响新创建的字符串。

   ```java
   char a[]={'H','e','l','l','o'};
   String sChar=new String(a,1,4);
   a[1]='s';
   ```

#### String转int

   String 字符串转整型 int 有以下两种方式：

- Integer.parseInt(str)
- Integer.valueOf(str).intValue()

```java
String str = "123";
int n = 0;
// String转int的两种方法
n = Integer.parseInt(str);
n = Integer.valueOf(str).intValue();
```

#### int转String

整型 int 转 String 字符串类型有以下 3 种方法：

- String s = String.valueOf(i);
- String s = Integer.toString(i);
- String s = "" + i;

使用第三种方法相对第一第二种耗时比较大。在使用第一种 valueOf() 方法时，注意 valueOf 括号中的值不能为空，否则会报空指针异常（NullPointerException）。

#### 字符串拼接

String 字符串虽然是不可变字符串，但也可以进行拼接只是会产生一个新的对象。String 字符串拼接可以使用“+”运算符或 String 的 concat(String str) 方法。“+”运算符优势是可以连接任何类型数据拼接成为字符串，而 concat 方法只能拼接 String 类型字符串。

#### 获取字符串长度

```java
String str = "Hello World!";
int length = str.length();
```

#### 字符串大小写转换

String 类的 toLowerCase() 方法可以将字符串中的所有字符全部转换成小写，而非字母的字符不受影响。

toUpperCase() 则将字符串中的所有字符全部转换成大写，而非字母的字符不受影响。

```java
String str="abcdef 我 ghijklmn";
System.out.println(str.toLowerCase());    // 输出：abcdef 我 ghijklmn
System.out.println(str.toUpperCase());    // 输出：ABCDEF 我 GHIJKLMN
```

#### 去除字符串中的空格

字符串中存在的首尾空格一般情况下都没有任何意义，如字符串“ Hello ”，但是这些空格会影响到字符串的操作，如连接字符串或比较字符串等，所以应该去掉字符串中的首尾空格，这需要使用 String 类提供的 trim() 方法。

```java
String str = " hello ";
System.out.println(str.length());    // 输出 7
System.out.println(str.trim().length());    // 输出 5
```

**如果不确定要操作的字符串首尾是否有空格，最好在操作之前调用该字符串的 trim() 方法去除首尾空格，然后再对其进行操作。**

#### 截取子字符串

1. substring(int beginIndex)

   此方式用于提取从索引位置开始至结尾处的字符串部分。调用时，括号中是需要提取字符串的开始位置，方法的返回值是提取的字符串。
   
   ```java
   String str = "我爱 Java 编程";
   String result = str.substring(3);
   System.out.println(result);    // 输出：Java 编程
   ```
   
2. substring(int beginIndex，int endIndex) 

   此方法中的 beginIndex 表示截取的起始索引，截取的字符串中包括起始索引对应的字符；endIndex 表示结束索引，截取的字符串中不包括结束索引对应的字符，如果不指定 endIndex，则表示截取到目标字符串末尾。该方法用于提取位置 beginIndex 和位置 endIndex 位置之间的字符串部分。

   ```java
   String day = "Today is Monday";    //原始字符串
       System.out.println("substring(0)结果："+day.substring(0));
       System.out.println("substring(2)结果："+day.substring(2));
       System.out.println("substring(10)结果："+day.substring(10));
       System.out.println("substring(2,10)结果："+day.substring(2,10));
       System.out.println("substring(0,5)结果："+day.substring(0,5));
   /*
       substring(0)结果：Today is Monday
       substring(2)结果：day is Monday
       substring(10)结果：onday
       substring(2,10)结果：day is M
       substring(0,5)结果：Today
   */
   ```

#### 分割字符串

String 类的 split() 方法可以按指定的分割符对目标字符串进行分割，分割后的内容存放在字符串数组中。

```java
str.split(String sign)
str.split(String sign,int limit)
```

其中它们的含义如下：

- str 为需要分割的目标字符串。
- sign 为指定的分割符，可以是任意字符串。
- limit 表示分割后生成的字符串的限制个数，如果不指定，则表示不限制，直到将整个目标字符串完全分割为止。

使用分隔符注意如下：

1) “.”和“|”都是转义字符，必须得加“\\”。

- 如果用“.”作为分隔的话，必须写成String.split("\\.")，这样才能正确的分隔开,不能String.split(".")。
- 如果用“|”作为分隔的话，必须写成String.split("\\|")，这样才能正确的分隔开，不能用`String.split("|")。

2) 如果在一个字符串中有多个分隔符，可以用“|”作为连字符，比如：“acount=? and uu =? or n=?”，把三个都分隔出来，可以用String.split("and|or")。

```java
 String Colors = "Red,Black,White,Yellow,Blue";
 String[] arr1 = Colors.split(","); // 不限制元素个数
 String[] arr2 = Colors.split(",", 3); // 限制元素个数为3
```

#### 字符串的替换

1. replace()

   replace() 方法用于将目标字符串中的指定字符（串）替换成新的字符（串）

   ```java
   String words = "hello java,hello php";
   words.replace("l","D")
   // 结果：heDDo java,heDDo php
   ```

2. replaceFirst()

   replaceFirst() 方法用于将目标字符串中匹配某正则表达式的第一个子字符串替换成新的字符串

   ```java
   String words = "hello java，hello php";
   String newStr = words.replaceFirst("hello","你好 ");
   System.out.println(newStr);    // 输出：你好 java,hello php
   ```

3. replaceAll()

   replaceAll() 方法用于将目标字符串中匹配某正则表达式的所有子字符串替换成新的字符串

   ```java
   String words = "hello java，hello php";
   String newStr = words.replaceAll("hello","你好 ");
   System.out.println(newStr);    // 输出：你好 java,你好 php
   ```

#### 字符串比较

1. equals()

   equals() 方法将逐个地比较两个字符串的每个字符是否相同。如果两个字符串具有相同的字符和长度，它返回 true，否则返回 false。对于字符的大小写，也在检查的范围之内。

   ```java
   String str1 = "abc";
   String str2 = new String("abc");
   String str3 = "ABC";
   System.out.println(str1.equals(str2)); // 输出 true
   System.out.println(str1.equals(str3)); // 输出 false
   ```

2. qualsignoreCase()

   equalsIgnoreCase() 方法的作用和语法与 equals() 方法完全相同，唯一不同的是 equalsIgnoreCase() 比较时不区分大小写。当比较两个字符串时，它会认为 A-Z 和 a-z 是一样的。

   ```java
   String str1 = "abc";
   String str2 = "ABC";
   System.out.println(str1.equalsIgnoreCase(str2));    // 输出 true
   ```

3. equals()与==的比较

   equals() 方法比较字符串对象中的字符。而==运算符比较两个对象引用看它们是否引用相同的实例。
   
   ```java
   String s1 = "Hello";
   String s2 = new String(s1);
   System.out.println(s1.equals(s2)); // 输出true
   System.out.println(s1 == s2); // 输出false
   ```
   
   变量 s1 指向由“Hello”创建的字符串实例。s2 所指的的对象是以 s1 作为初始化而创建的。因此这两个字符串对象的内容是一样的。但它们是不同的对象，这就意味着 s1 和 s2 没有指向同一的对象，因此它们是不相等的。

4. compareTo()

   compareTo() 方法用于按字典顺序比较两个字符串的大小，该比较是基于字符串各个字符的 Unicode 值。

   ```java
   str.compareTo(String otherstr);
   /*
      如果按字典顺序 str 位于 otherster 参数之前，比较结果为一个负整数；如	果 str 位于 otherstr 之后，比较结果为一个正整数；如果两个字符串相等，	 则结果为 0。
   */
   ```

   **如果两个字符串调用 equals() 方法返回 true，那么调用 compareTo() 方法会返回 0。**

#### Stringbuffer

StringBuffer 类可以比 String 类更高效地处理字符串。

因为 StringBuffer 类是可变字符串类，创建 StringBuffer 类的对象后可以随意修改字符串的内容。每个 StringBuffer 类的对象都能够存储指定容量的字符串，如果字符串的长度超过了 StringBuffer 类对象的容量，则该对象的容量会自动扩大。

StringBuffer 类提供了 3 个构造方法来创建一个字符串，如下所示：

- StringBuffer() 构造一个空的字符串缓冲区，并且初始化为 16 个字符的容量。
- StringBuffer(int length) 创建一个空的字符串缓冲区，并且初始化为指定长度 length 的容量。
- StringBuffer(String str) 创建一个字符串缓冲区，并将其内容初始化为指定的字符串内容 str，字符串缓冲区的初始容量为 16 加上字符串 str 的长度。

```java
// 定义一个空的字符串缓冲区，含有16个字符的容量
StringBuffer str1 = new StringBuffer();
// 定义一个含有10个字符容量的字符串缓冲区
StringBuffer str2 = new StringBuffer(10);
// 定义一个含有(16+4)的字符串缓冲区，"青春无悔"为4个字符
StringBuffer str3 = new StringBuffer("青春无悔");
/*
*输出字符串的容量大小
*capacity()方法返回字符串的容量大小
*/
System.out.println(str1.capacity());    // 输出 16
System.out.println(str2.capacity());    // 输出 10
System.out.println(str3.capacity());    // 输出 20
```

1. append()

   StringBuffer 类的 append() 方法用于向原有 StringBuffer 对象中追加字符串。追加内容到当前 StringBuffer 对象的末尾。

   ```java
   StringBuffer buffer = new StringBuffer("hello,");    // 创建一个 StringBuffer 对象
   String str = "World!";
   buffer.append(str);    // 向 StringBuffer 对象追加 str 字符串
   System.out.println(buffer.substring(0));    // 输出：Hello,World!
   ```

2. setCharAt()

   StringBuffer 类的 setCharAt() 方法用于在字符串的指定索引位置替换一个字符。

   ```java
   StringBuffer sb = new StringBuffer("hello");
   sb.setCharAt(1,'E');
   System.out.println(sb);    // 输出：hEllo
   sb.setCharAt(0,'H');
   System.out.println(sb);    // 输出：HEllo
   sb.setCharAt(2,'p');
   System.out.println(sb);    // 输出：HEplo
   ```

3. reverse() 

   StringBuffer 类中的 reverse() 方法用于将字符串序列用其反转的形式取代。

   ```java
   StringBuffer sb = new StringBuffer("java");
   sb.reverse();
   System.out.println(sb);    // 输出：avaj
   ```

4. deleteCharAt() 和 delete() 

   deleteCharAt() 方法用于移除序列中指定位置的字符

   ```java
   StringBuffer sb = new StringBuffer("She");
   sb.deleteCharAt(2);
   System.out.println(sb);    // 输出：Sh
   ```

   delete() 方法用于移除序列中子字符串的字符

   ```java
   StringBuffer sb = new StringBuffer("hello jack");
   sb.delete(2,5);
   System.out.println(sb);    // 输出：he jack
   sb.delete(2,5);
   System.out.println(sb);    // 输出：heck
   ```

### String, StringBuffer, StringBuilder类的区别

StringBuilder 和 StringBuffer 功能基本相似，方法也差不多。不同的是，StringBuffer 是线程安全的，而 StringBuilder 则没有实现线程安全功能，所以性能略高。因此在通常情况下，如果需要创建一个内容可变的字符串对象，则应该优先考虑使用 StringBuilder 类。

### 数组

数组（array）是一种最简单的复合数据类型，它是有序数据的集合，数组中的每个元素具有相同的数据类型，可以用一个统一的数组名和不同的下标来确定数组中唯一的元素。根据数组的维度，可以将其分为一维数组、二维数组和多维数组等。

在计算机语言中数组是非常重要的集合类型，大部分计算机语言中数组具有如下三个基本特性：

1.  一致性：数组只能保存相同数据类型元素，元素的数据类型可以是任何相同的数据类型。
2. 有序性：数组中的元素是有序的，通过下标访问。
3. 不可变性：数组一旦初始化，则长度（数组中元素的个数）不可变。

总的来说，数组具有以下特点：

- 数组可以是一维数组、二维数组或多维数组。
- 数值数组元素的默认值为 0，而引用元素的默认值为 null。
- 数组的索引从 0 开始，如果数组有 n 个元素，那么数组的索引是从 0 到（n-1）。
- 数组元素可以是任何类型，包括数组类型。
- 数组类型是从抽象基类 Array 派生的引用类型。

#### 声明数组

```java
int[] score;    // 存储学生的成绩，类型为整型
double[] price;    // 存储商品的价格，类型为浮点型
String[] name;    // 存储商品名称，类型为字符串型
```

**在声明数组时不需要规定数组的长度**

```java
int score[10]; // 这是错误的
```

声明了数组，只是得到了一个存放数组的变量，并没有为数组元素分配内存空间，不能使用。在 Java 中可以使用 new 关键字来给数组分配空间。

```java
score = new int[10];
price = new double[30];
name = new String[20];
```

一旦声明了数组的大小，就不能再修改。这里的数组长度也是必需的，不能少。

数组在初始化数组的同时，可以指定数组的大小，也可以分别初始化数组中的每一个元素。

1. 使用 new 指定数组大小后进行初始化

   ```java
   int[] number = new int[5];
   number[0] = 1;
   number[1] = 2;
   number[2] = 3;
   number[3] = 5;
   number[4] = 8;
   ```

2. 使用 new 指定数组元素的值

   ```java
   int[] number = new int[]{1, 2, 3, 5, 8};
   ```

3. 直接指定数组元素的值

   ```java
   int[] number = {1, 2, 3, 4, 5};
   ```

   使用这种方式时，数组的声明和初始化操作要同步，即不能省略数组变量的类型。

   ```java
   // 这是错误的
   int[] number;
   number = {1,2,3,5,8};
   ```

### class

```java
[public][abstract|final]class<class_name>[extends<class_name>][implements<interface_name>] {
    // 定义属性部分
    <property_type><property1>;
    <property_type><property2>;
    <property_type><property3>;
    …
    // 定义方法部分
    function1();
    function2();
    function3();
    …
}
```

上述语法中各关键字的描述如下。

- `public`：表示“共有”的意思。如果使用 public 修饰，则可以被其他类和程序访问。每个 Java 程序的主类都必须是 public 类，作为公共工具供其他类和程序使用的类应定义为 public 类。
- `abstract`：如果类被 abstract 修饰，则该类为抽象类，抽象类不能被实例化，但抽象类中可以有抽象方法（使用 abstract 修饰的方法）和具体方法（没有使用 abstract 修饰的方法）。继承该抽象类的所有子类都必须实现该抽象类中的所有抽象方法（除非子类也是抽象类）。
- `final`：如果类被 final 修饰，则不允许被继承。
- `class`：声明类的关键字。
- `class_name`：类的名称。
- `extends`：表示继承其他类。
- `implements`：表示实现某些接口。
- `property_type`：表示成员变量的类型。
- `property`：表示成员变量名称。
- `function()`：表示成员方法名称。

#### 成员变量

```java
[public|protected|private][static][final]<type><variable_name>
```

各参数的含义如下。

- public、protected、private：用于表示成员变量的访问权限。
- static：表示该成员变量为类变量，也称为静态变量。
- final：表示将该成员变量声明为常量，其值无法更改。
- type：表示变量的类型。
- variable_name：表示变量名称。

#### 成员方法

```java
[public|private|protected][static]<void|return_type><method_name>([paramList]) {
        // 方法体
}
```

- public、private、protected：表示成员方法的访问权限。
- static：表示限定该成员方法为静态方法。
- final：表示限定该成员方法不能被重写或重载。
- abstract：表示限定该成员方法为抽象方法。抽象方法不提供具体的实现，并且所属类型必须为抽象类。

### public, private, protected,friendly

|     访问范围     | private  | friendly(默认) | protected | public |
| :--------------: | :------: | :------------: | :-------: | :----: |
|     同一个类     |  可访问  |     可访问     |  可访问   | 可访问 |
| 同一包中的其他类 | 不可访问 |     可访问     |  可访问   | 可访问 |
|  不同包中的子类  | 不可访问 |    不可访问    |  可访问   | 可访问 |
| 不同包中的非子类 | 不可访问 |    不可访问    | 不可访问  | 可访问 |

1. private

   用 private 修饰的类成员，只能被该类自身的方法访问和修改，而不能被任何其他类（包括该类的子类）访问和引用。

2. friendly

   如果一个类没有访问控制符，说明它具有默认的访问控制特性。这种默认的访问控制权规定，该类只能被同一个包中的类访问和引用，而不能被其他包中的类使用，即使其他包中有该类的子类。这种访问特性又称为包访问性（package private）。

3. protected

   用保护访问控制符 protected 修饰的类成员可以被三种类所访问：该类自身、与它在同一个包中的其他类以及在其他包中的该类的子类。

4. public

   当一个类被声明为 public 时，它就具有了被其他包中的类访问的可能性，只要包中的其他类在程序中使用 import 语句引入 public 类，就可以访问和引用这个类。

### final

使用 final 关键字声明类、变量和方法需要注意以下几点：

- final 用在变量的前面表示变量的值不可以改变，此时该变量可以被称为常量。
- final 用在方法的前面表示方法不可以被重写（子类中如果创建了一个与父类中相同名称、相同返回值类型、相同参数列表的方法，只是方法体中的实现不同，以实现不同于父类的功能，这种方式被称为方法重写，又称为方法覆盖。这里了解即可，教程后面我们会详细讲解）。
- final 用在类的前面表示该类不能有子类，即该类不可以被继承。

#### final修饰变量

final 修饰的变量即成为常量，只能赋值一次，但是 final 所修饰局部变量和成员变量有所不同。

1. final 修饰的局部变量必须使用之前被赋值一次才能使用。
2. final 修饰的成员变量在声明时没有赋值的叫“空白 final 变量”。空白 final 变量必须在构造方法或静态代码块中初始化。

#### final修饰引用类型变量

当使用 final 修饰基本类型变量时，不能对基本类型变量重新赋值，因此基本类型变量不能被改变。 但对于引用类型变量而言，它保存的仅仅是一个引用，final 只保证这个引用类型变量所引用的地址不会改变，即一直引用同一个对象，但这个对象完全可以发生改变。

### 封装

封装的特点：

- 只能通过规定的方法访问数据。
- 隐藏类的实例细节，方便修改和实现。

实现封装的具体步骤如下：

1. 修改属性的可见性来限制对属性的访问，一般设为 private。
2. 为每个属性创建一对赋值（setter）方法和取值（getter）方法，一般设为 public，用于属性的读写。
3. 在赋值和取值方法中，加入属性控制语句（对属性值的合法性进行判断）。

### 继承

```java
public class People {
    public String name; // 姓名
    public int age; // 年龄
    public String sex; // 性别
    public String sn; // 身份证号
    public People(String name, int age, String sex, String sn) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.sn = sn;
    }
    public String toString() {
        return "姓名：" + name + "\n年龄：" + age + "\n性别：" + sex + "\n身份证号：" + sn;
    }
}

// 继承关键字 extends
public class Student extends People {
    private String stuNo; // 学号
    private String department; // 所学专业
    public Student(String name, int age, String sex, String sn, String stuno, String department) {
        super(name, age, sex, sn); // 调用父类中的构造方法
        this.stuNo = stuno;
        this.department = department;
    }
    public String toString() {
        return "姓名：" + name + "\n年龄：" + age + "\n性别：" + sex + "\n身份证号：" + sn + "\n学号：" + stuNo + "\n所学专业：" + department;
    }
}
```

**`如果在父类中存在有参的构造方法而并没有重载无参的构造方法，那么在子类中必须含有有参的构造方法，因为如果在子类中不含有构造方法，默认会调用父类中无参的构造方法，而在父类中并没有无参的构造方法，因此会出错。`**

使用继承的注意点：

1. 子类一般比父类包含更多的属性和方法。
2. 父类中的 private 成员在子类中是不可见的，因此在子类中不能直接使用它们。
3. 父类和其子类间必须存在“是一个”即“is-a”的关系，否则不能用继承。但也并不是所有符合“is-a”关系的都应该用继承。例如，正方形是一个矩形，但不能让正方形类来继承矩形类，因为正方形不能从矩形扩展得到任何东西。正确的继承关系是正方形类继承图形类。
4. Java 只允许单一继承（即一个子类只能有一个直接父类），C++ 可以多重继承（即一个子类有多个直接父类）。

#### 继承的优缺点

优点：

1. 实现代码共享，减少创建类的工作量，使子类可以拥有父类的方法和属性。
2. 提高代码维护性和可重用性。
3. 提高代码的可扩展性，更好的实现父类的方法。

缺点：

1. 继承是侵入性的。只要继承，就必须拥有父类的属性和方法。
2. 降低代码灵活性。子类拥有父类的属性和方法后多了些约束。
3. 增强代码耦合性（开发项目的原则为高内聚低耦合）。当父类的常量、变量和方法被修改时，需要考虑子类的修改，有可能会导致大段的代码需要重构。

### super

super 关键字的功能：

- 在子类的构造方法中显式的调用父类构造方法
- 访问父类的成员方法和变量。

**`super( ) 必须是在子类构造方法的方法体的第一行。`**

在继承关系中，由于在子类的构造方法中，第一条语句默认为调用父类的无参构造方法（即默认为 super()，一般这行代码省略了）。所以当在父类中定义了有参构造方法，但是没有定义无参构造方法时，编译器会强制要求我们定义一个相同参数类型的构造方法。

#### super 和 this

super 关键字的用法：

- super.父类属性名：调用父类中的属性
- super.父类方法名：调用父类中的方法
- super()：调用父类的无参构造方法
- super(参数)：调用父类的有参构造方法

this 关键字的用法：

- this.属性名：表示当前对象的属性
- this.方法名(参数)：表示调用当前对象的方法

### 对象类型转换

#### 向上转型

父类引用指向子类对象为向上转型。

```java
fatherClass obj = new sonClass();
//fatherClass 是父类名称或接口名称，obj 是创建的对象，sonClass 是子类名称。
```

向上转型就是把子类对象直接赋给父类引用，不用强制转换。使用向上转型可以调用父类类型中的所有成员，不能调用子类类型中特有成员，最终运行效果看子类的具体实现。

#### 向下转型

与向上转型相反，子类对象指向父类引用为向下转型。

```java
sonClass obj = (sonClass) fatherClass;
// fatherClass 是父类名称，obj 是创建的对象，sonClass 是子类名称。
```

通过引用类型变量来访问所引用对象的属性和方法时，Java 虚拟机将采用以下绑定规则：

- 实例方法与引用变量实际引用的对象的方法进行绑定，这种绑定属于动态绑定，因为是在运行时由 Java 虚拟机动态决定的。例如，animal.eat() 是将 eat() 方法与 Cat 类绑定。
- 静态方法与引用变量所声明的类型的方法绑定，这种绑定属于静态绑定，因为是在编译阶段已经做了绑定。例如，animal.staticEat() 是将 staticEat() 方法与 Animal 类进行绑定。
- 成员变量（包括静态变量和实例变量）与引用变量所声明的类型的成员变量绑定，这种绑定属于静态绑定，因为在编译阶段已经做了绑定。例如，animal.name 和 animal.staticName 都是与 Animal 类进行绑定。

#### 向下转型成员变量与方法

向下转型时：

```java
Father father = new Son();

/**
	1. 不能访问子类特有的方法与成员变量
	2. 访问共有的方法时会被子类方法覆盖
	3. 访问共有变量时会访问父类的成员变量
	4. 要访问子类的特有方法与成员变量需要向下转型((Son)father).方法/  ((Son)father).变量名
	5. 静态变量与静态方法会调用父类的
*/
```

### 内部类

```javascript
public class Test {
    public class InnerClass {
        public int getSum(int x,int y) {
            return x + y;
        }
    }

    public static void main(String[] args) {
        Test.InnerClass ti = new Test().new InnerClass();
        int i = ti.getSum(2/3);
        System.out.println(i);    // 输出5
    }
}
```

- 内部类拥有外部类的所有元素的访问权限。

- 外部类可以能过new 的方法直接访问内部类

  ```java
  InnerClass ic = new InnerClass();    // InnerClass为内部类的类名
  ```

  

- 外部类以外的其他类中刚需要通过内部类的完整类名访问

  ```java
  Test.InnerClass ti = newTest().new InnerClass(); 
  ```

### 集合

#### ArrayList

ArrayList 类实现了可变数组的大小，存储在内的数据称为元素。它还提供了快速基于索引访问元素的方式，对尾部成员的增加和删除支持较好。使用 ArrayList 创建的集合，允许对集合中的元素进行快速的随机访问，不过，向 ArrayList 中插入与删除元素的速度相对较慢。

ArrayList 类的常用构造方法有如下两种重载形式：

- ArrayList()：构造一个初始容量为 10 的空列表。
- ArrayList(Collection<?extends E>c)：构造一个包含指定 Collection 元素的列表，这些元素是按照该 Collection 的迭代器返回它们的顺序排列的。

#### LinkedList

LinkedList 类采用链表结构保存对象，这种结构的优点是便于向集合中插入或者删除元素。需要频繁向集合中插入和删除元素时，使用 LinkedList 类比 ArrayList 类效果高，但是 LinkedList 类随机访问元素的速度则相对较慢。这里的随机访问是指检索集合中特定索引位置的元素。

#### ArrayList 与 LinkedList区别

ArrayList 与 LinkedList 都是 List 接口的实现类，因此都实现了 List 的所有未实现的方法，只是实现的方式有所不同。

ArrayList 是基于动态数组[数据结构](http://c.biancheng.net/data_structure/)的实现，访问元素速度优于 LinkedList。LinkedList 是基于链表数据结构的实现，占用的内存空间比较大，但在批量插入或删除数据时优于 ArrayList。

对于快速访问对象的需求，使用 ArrayList 实现执行效率上会比较好。需要频繁向集合中插入和删除元素时，使用 LinkedList 类比 ArrayList 类效果高。

#### HashSet

HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时就是使用这个实现类。HashSet 是按照 Hash 算法来存储集合中的元素。因此具有很好的存取和查找性能。

HashSet 具有以下特点：

- 不能保证元素的排列顺序，顺序可能与添加顺序不同，顺序也有可能发生变化。
- HashSet 不是同步的，如果多个线程同时访问或修改一个 HashSet，则必须通过代码来保证其同步。
- 集合元素值可以是 null。

#### TreeSet

TreeSet 类同时实现了 Set 接口和 SortedSet 接口。SortedSet 接口是 Set 接口的子接口，可以实现对集合进行自然排序，因此使用 TreeSet 类实现的 Set 接口默认情况下是自然排序的，这里的自然排序指的是升序排序。

TreeSet 只能对实现了 Comparable 接口的类对象进行排序，因为 Comparable 接口中有一个 compareTo(Object o) 方法用于比较两个对象的大小。例如 a.compareTo(b)，如果 a 和 b 相等，则该方法返回 0；如果 a 大于 b，则该方法返回大于 0 的值；如果 a 小于 b，则该方法返回小于 0 的值。

### HashMap & TreeMap

Map 是一种键-值对（key-value）集合，Map 集合中的每一个元素都包含一个键（key）对象和一个值（value）对象。用于保存具有映射关系的数据。

Map 集合里保存着两组值，一组值用于保存 Map 里的 key，另外一组值用于保存 Map 里的 value，key 和 value 都可以是任何引用类型的数据。Map 的 key 不允许重复，value 可以重复，即同一个 Map 对象的任何两个 key 通过 equals 方法比较总是返回 false。

TreeMap 类的使用方法与 HashMap 类相同，唯一不同的是 TreeMap 类可以对键对象进行排序

### MAP的遍历

1. 在 for 循环中使用 entries 实现 Map 的遍历（最常见和最常用的）。

```java
public static void main(String[] args) {
    Map<String, String> map = new HashMap<String, String>();
    map.put("Java入门教程", "http://c.biancheng.net/java/");
    map.put("C语言入门教程", "http://c.biancheng.net/c/");
    for (Map.Entry<String, String> entry : map.entrySet()) {
        String mapKey = entry.getKey();
        String mapValue = entry.getValue();
        System.out.println(mapKey + "：" + mapValue);
    }
}
```

2. 使用 for-each 循环遍历 key 或者 values，一般适用于只需要 Map 中的 key 或者 value 时使用。性能上比 entrySet 较好。

```java
Map<String, String> map = new HashMap<String, String>();
map.put("Java入门教程", "http://c.biancheng.net/java/");
map.put("C语言入门教程", "http://c.biancheng.net/c/");
// 打印键集合
for (String key : map.keySet()) {
    System.out.println(key);
}
// 打印值集合
for (String value : map.values()) {
    System.out.println(value);
}
```

3. 使用迭代器（Iterator）遍历

```java
Map<String, String> map = new HashMap<String, String>();
map.put("Java入门教程", "http://c.biancheng.net/java/");
map.put("C语言入门教程", "http://c.biancheng.net/c/");
Iterator<Entry<String, String>> entries = map.entrySet().iterator();
while (entries.hasNext()) {
    Entry<String, String> entry = entries.next();
    String key = entry.getKey();
    String value = entry.getValue();
    System.out.println(key + ":" + value);
}
```

4. 通过键找值遍历，这种方式的效率比较低，因为本身从键取值是耗时的操作。

```java
for(String key : map.keySet()){
    String value = map.get(key);
    System.out.println(key+":"+value);
}
```

### 反射机制

优点：

- 能够运行时动态获取类的实例，大大提高系统的灵活性和扩展性。
- 与 Java 动态编译相结合，可以实现无比强大的功能。
- 对于 Java 这种先编译再运行的语言，能够让我们很方便的创建灵活的代码，这些代码可以在运行时装配，无需在组件之间进行源代码的链接，更加容易实现面向对象。


缺点：

- 反射会消耗一定的系统资源，因此，如果不需要动态地创建一个对象，那么就不需要用反射；
- 反射调用方法时可以忽略权限检查，获取这个类的私有方法和属性，因此可能会破坏类的封装性而导致安全问题。