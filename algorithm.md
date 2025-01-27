## 算法

### 衡量算法的好坏

#### 时间复杂度

- 渐进时间复杂度

  若存在函数f(n)，使得当n趋近于无穷大时，T(n)/f(n)的极值为不等于零的常数，则称f(n)是T(n)的同数量级函数。记住T(n) = O(f(n))，称为O(f(n))，O为算法的渐进时间复杂度，简称为时间复杂度。

##### 时间复杂度推导原则

- 如果运行时间是常数量级，则用常数1表示
- 只保留时间函数中的最高阶项
- 如果最高阶项存在，则省去最高阶项前面的系数

```shell
# 例如T(n) = 3n
# 最高阶项为3n,省去系数3，则转化的时间复杂度为Tn = O(n)

# T(n) = 5logn
# 最高阶项为5logn,省去系数5，则转化的时间复杂度为T(n) = O(logn)

# T(n) = 2
# 只有常数量级，则转化的时间复杂度为 T(n) = O(1)

# T(n) = 0.5n² + 0.5n
# 最高阶项为0.5n²，省去系数0.5，则转化的时间复杂度为T(n) = O(n²)

```



#### 空间复杂度

- 时间复杂度相同的情况下，算法占用的内存空间自然是越小越好。描述一个算法占用的内存空间大小的指标就是称为空间复杂度。公式记作S(n) = O(f(n))，其中n为问题的规模，f(n)为算法所占存储空间的函数。

1. **常量空间**

   - 当算法的存储空间大小固定，和输入规模没有直接的关系时，空间复杂度记作O(1)

   ```python
   def fun(n):
       i = 3
       # do something
   ```

2. **线性空间**

   - 当算法分配的空间是一个线性的集合（如列表），并且集合大小和输入规模n成正比时，空间复杂度记作O(n)。

   ```python
   def fun(n):
       array = [[0] * n]
       # do something
   ```

3. **二维空间**

   - 当算法分配的空间是一个二维列表集合，并且集合的长度和宽度都与输入规模n成正比时，空间复杂度记作O(n²)

   ```python
   def fun(n):
       matrix = [[0] * n] * n
       # do something
   ```

4. **递归空间**

   - 递归是一个比较特殊的场景。虽然递归代码中没有显式地声明变量或集合，但是计算机在执行程序时，会专门分配一块内存，用来存储“函数调用栈”。

     “函数调用栈”包括**进栈**和**出栈**两个行为。

   ```python
   def fun(n):
       if n > 0:
          fun(n - 1)
       # do something
   ```

### 数据结构

- 是数据的的组织，管理和存储格式，其使用目的是高效地访问和修改数据

#### 线性结构

- 线性结构是简单的数据结构，包括数组，链表，以及由它们衍生出来的栈，队列，哈希表

#### 树

- 树是相对复杂的数据结构，其中比较有代表性的是二叉树，由它又衍生出了二叉堆之类的数据结构

#### 图

- 图更为复杂的数据结构，因为在图中会呈现出多对多的关联关系

#### 其他数据结构

- 由上述基本数据结构变形而来，用于解决某些特定的问题，如跳表，哈希链表，位图等

### 树

- 树是n(n >=0)个节点的有限集。当n=0时，称为空树，在任意一个非空树中，有如下特点：
  1. 有且仅有一个特定的称为根的节点。
  2. 当n>1时，其余节点可分为m(m>0)个互不相交的有限集，每一个集合本身又是一个树，并称为根的子树

#### 二叉树

- 二叉树是树的一种特殊形式。这种树的每个节点最多有**2个**孩子节点。这里的最多有2个，也可能只有1个，或者没有孩子节点。
- 二叉树的两个节点，一个被称为左孩子，一个被称为右孩子。

##### 满二叉树

- 一个二叉树的所有非叶子节点都存在左孩子和右孩子，并且所有叶子节点都在同一层级上，那么这个对就是满二叉树。简单点说满二叉树的每一个节点都是满的。

##### 完全二叉树

- 对一个有n个节点的二叉树，按层级顺序编号，则所有节点的编号为从1到n。如果这个树所有节点和同样尝试的满二叉树的编号为从1到n的节点位置相同，则这个二叉树为完全二叉树。

##### 二叉树的应用

- 主要的应用还在于进行查找操作和维持相对顺序

##### 二叉查找树

- 在二叉树的基础上增加了以下几个条件

1. 如果左子树不为空，则左子树上所有节点的值均小于根节点的值
2. 如果右子树不为空，则右子树上所有节点的 值均大于根节点的值
3. 左子树，右子树也都是二叉查找树

- 对于一个节点分布相对均衡的二叉查找树来说，如果节点总数是n，那么搜索节点的时间复杂度就是O(logn)，和树的深度是一样的

##### 维持相对顺序

- 二叉查找树要求左子树节点的值小于父节点值，右子树节点的值大于父节点的值，正是这样保证了二叉树的有序性。因些二叉查找树还有另一个名字———二叉排序树