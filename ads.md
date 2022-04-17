## AVL树

### 什么是AVL树

AVL树又称为高度平衡的二叉搜索树，即自平衡二叉搜索树（Self-balancing binary search tree）

它能保持二叉树的高度平衡，尽量降低二叉树的高度，减少树的平均搜索长度，是二叉搜索树的一种改进算法

AVL树的查找、插入和删除在**平均和最坏情况下都是O(log n)**

### 平衡因子(BF，Balance Factor)

- 定义
  - 每个节点的左子树的高度减去右子树的高度
- 计算公式
  - 即BF(node) = h~L~-h~R~
  - 在一棵AVL树中，BF(node)= -1,0,1

### 高度平衡（height balanced）

1. **规定**空树是高度平衡的且空树的高度为-1

2. 如果T是一颗非空的树，T~L~和T~R~是其左子树和右子树，则当满足以下条件时T是高度平衡的
   1. T~L~和T~R~是高度平衡的
   2. 平衡因子的绝对值即| h~L~-h~R~ |<=1，其中h~L~和h~R~分别是T~L~和T~R~的高度

### 旋转规则

#### 添加节点

AVL树当且仅当检测到某一节点的平衡因子出错时进行旋转

下方令插入节点后平衡因子出错的节点为X，插入的节点为K

> 注意：X不一定是插入节点的爷爷节点，还可能是其他祖先节点

| 情况 | 说明                           | 处理方式                                     |
| ---- | ------------------------------ | -------------------------------------------- |
| LL   | 节点K插入到节点X的左孩子的左边 | 对节点X进行右旋                              |
| RR   | 节点K插入到节点X的右孩子的右边 | 对节点X进行左旋                              |
| LR   | 节点K插入到节点X的左孩子的右边 | 先对节点X的左孩子进行左旋，再对节点X进行右旋 |
| RL   | 节点K插入到节点X的右孩子的左边 | 先对节点X的右孩子进行右旋，再对节点X进行左旋 |

##### 示例

- LL

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123104112780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW9fbGVpbGVp,size_16,color_FFFFFF,t_70)

- RR

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123111142437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW9fbGVpbGVp,size_16,color_FFFFFF,t_70)

- LR

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123115056269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW9fbGVpbGVp,size_16,color_FFFFFF,t_70)

- RL

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123212800497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW9fbGVpbGVp,size_16,color_FFFFFF,t_70)

### AVL树的最少节点数

当N~h~ = N~h-1~ + N~h-2~ + 1 时，高度平衡树所需的节点数最少，此时左右子树高度不同，其中 N~h~ 为高度为 h 的高度平衡树的最少节点数，h > 0

### 删除节点

同二叉搜索树，即遵循以下步骤（以删除X节点为例）：

1. 找到X节点

2. 删除X节点

3. 找到X节点的左子树的最大值，设为Y

4. 用节点Y代替原先X节点的位置，即成为新的根
5. 检查是否需要维护平衡因子

## 伸展树（Splay Tree）

伸展树的均摊时间复杂度是O(MlogN)

### 基本思想

考虑到局部性原理（刚被访问的内容下次可能仍会被访问，查找次数多的内容可能下一次会被访问），为了使整个查找时间更小，被查频率高的那些节点应当经常处于靠近树根的位置。

伸展树不需要保存节点的高度信息，这样能够节省存储空间。

### 旋转规则

#### 插入操作

splay树插入的时候按照二叉搜索树规则插入，然后对插入的节点进行一次查找操作

> 查找操作的目的是将新插入的节点提升至根节点

#### 查找操作

以被访问的节点X为例，假设节点P为节点X的当前父亲，节点G为X的当前爷爷

| 情况      | 调整方法            |
| --------- | ------------------- |
| P是根节点 | 直接旋转P和X        |
| Zig-zag   | 依次旋转P和X、G和X  |
| Zig-zig   | 绕着节点X做一次单旋 |

重复进行上述操作，直到X成为新的根节点

> Zig-zag情况包含LR和RL，例如：
>
> ![image-20220415123955634](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415123955634.png)
>
> Zig-zig情况包含LL和RR，例如：
>
> ![image-20220415124029704](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415124029704.png)

> 注意：以上所有旋转均遵循二叉搜索树性质

#### 删除操作

1. 对X节点进行一次查找操作
   - 查找后X就成为了根节点
2. 删除X节点
   - 删去X节点后剩下左子树T~L~和右子树T~R~
3. 找到左子树T~L~的最大值节点，令该节点作为左子树的根
4. 令T~R~称为修改后的左子树根节点的右孩子
   - 即成为左子树最大值的右孩子

## 平摊分析（Amortized Analysis）

### 什么是平摊分析

1. 平摊分析是指在某种数据结构上完成一系列操作时，最坏情况下的平均时间

2. 平摊分析与传统分析方法的主要差别为：
   - 平摊分析时间与传统分析方法的平均情况下时间不同，它是最坏情况下的平均时间
   - 平摊分析不涉及概率分析，即它只分析总体
   - 平摊分析中时间函数T(n)，其中n指的是操作的次数，而不是输入的规模。

3. 常见的有三种方法：聚集法、记账法、势能法。

### 聚合分析（Aggregate Analysis）

#### 思想

聚合分析是把n个不同或相同的操作合在一起加以分析计算得到总时间的方法

即 n 个操作序列在最坏情况下的总时间记为T(n)，其中n为操作数。

由此得到在最坏情况下每个操作的平均时间为T(n)/n。

#### 样例

以push、pop和multipop操作为例

push操作所需时间为1，pop操作所需时间为1，multipop操作所需时间为min(sizeof(S),k)，其中S为栈中剩余元素，k为出栈次数

则由聚合分析的思想可得：由于pop操作和multipop操作均需由push次数决定，在最差情况下将进行n/2次push和n/2次multipop，则时间T=n，故每个操作的平摊时间为T(n)/n=O(1)

### 会计分析（Accounting Analysis）

#### 思想

会计分析也称为记账分析，分析思想是先对每个不同的操作核定一个不同的费用（时间），然后计算n次操作总的费用。

由于所得费用是人为规定的，因此会出现得到松上界的情况。使得所得的操作费用可能与实际费用不符

对于松上界存在二种情况：

1. 超额收费：核定费用>实际费用，此时差额存放在该对象的身上。

2. 收费不足：核定费用<实际费用，此时差额由该对象身上的存款支付

费用的核定(平摊核定)是否正确需检查n次操作核定总费用是否大于n次操作实际总费用。因此，在记账方法中，某些操作的费用比它们实际代价或多或少，但所求得的平摊时间复杂度是相同的

#### 样例

以push、pop和multipop操作为例

由于push操作决定了另外两种操作的操作次数，则可令push操作费用为+2，则pop和multipop操作的费用为0（该费用由push承担了），故平摊时间仍为O(1)

### 势能分析（Potential method）

#### 思想

势能分析通过定义一个势能函数，用于评价数据结构某个状态的势能，把每个操作的时间复杂度加上操作导致的势能变化作为摊还复杂度，如果经过了一系列操作以后，势能不减少，这一系列操作的时间复杂度之和不大于这一系列操作的摊还复杂度之和。

第i个操作的摊还代价C~i~^’^ = C~i~(第i个操作的实际代价) + 当前数据结构的总势能 - 进行第i个操作之前的数据结构的总势能，即

![image-20220415160001755](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415160001755.png)

则

```
总摊还代价（即摊还复杂度）= 所有操作的实际代价（实际时间复杂度）+ 进行所有操作后的数据结构总势能 - 原数据结构总势能
```

即

![image-20220415160030040](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415160030040.png)

如果势能函数满足`数据结构总势能 - 原数据结构总势能 >= 0`，则总摊还代价是总实际代价的一个上界。

其中势能是由势能函数所求得的。而势能函数是由使用者根据实际情况所规定的。

总的来说，一个好的势能函数应该总是假设它的最小值在序列的开头

#### 样例

略

## 红黑树（Red-Black Trees）

### 红黑树的性质

红黑树是一棵满足下列红黑属性的二叉搜索树

1. 每个节点要么是红的要么是黑的

2. 根节点是黑的

3. 每个叶节点（即空节点）是黑的

4. 如果一个节点还是红的，那么他的孩子都是黑的

5. 对于每个节点，从该节点到子代叶节点的所有的最短路径包含相同的黑节点数

红黑树**可以在O(log n)时间内做查找，插入和删除**

红黑树从根到叶子的最长路径不会超过最短路径的2倍

### 黑高（black-height）

黑高是从x（不包含x）到任意子代叶节点的黑节点的数量，节点x的黑高记为 bh(x)。其中bh(Tree) = bh(root)

### 调整规则

#### 插入节点

插入节点时将新插入的节点视为红色进行插入

| 代码 | 情况                                                         | 调整方法                                                     | 口诀                           |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------ |
| 1    | 新节点即根节点                                               | 将新节点变色为黑色                                           | 新根即黑                       |
| 2    | 新节点的父节点是黑色                                         | 将插入的新节点设为红色即可                                   | 父黑调红                       |
| 3    | 新节点的父节点和叔节点都是红色                               | 将新节点的父节点和叔节点变为黑色，将新节点的祖父节点变为红色，之后从祖父节点开始依次检查上去，直到整棵树均满足红黑树规则 | 父叔均红爷变红                 |
| 4    | 新节点的父节点是红色，叔节点是黑色或者没有叔节点，且祖父节点到新节点的关系为左右 | 以父节点为轴，做一次左旋，变为情况5，然后根据情况5进行处理即可 | 父红叔黑爷左右，父旋完后再处理 |
| 5    | 新节点的父节点是红色，叔节点是黑色或者没有叔节点，且祖父节点到新节点的关系为左左 | 以祖父节点为轴，进行一次右旋，使原先的父节点成为祖父节点，然后将原来的祖父节点变为红色，将原来的父节点变为黑色 | 父红叔黑爷左左，爷旋红，父变黑 |
| 6    | 右左，为情况4的镜像                                          | 略                                                           |                                |
| 7    | 右右，为情况5的镜像                                          | 略                                                           |                                |

#### 删除节点（未梳理完）

参考[红黑树删除操作 - Jacc.Kim - 博客园 (cnblogs.com)](https://www.cnblogs.com/tongy0/p/5460623.html)

| 代码 | 情况                                                         | 调整方法                                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | 如果待删除节点有两个非空的孩子节点                           | 用右子树中最小的节点替代待删除节点，并将该节点的颜色变成待删除节点的颜色，将原问题转化成其他情况的问题 |
| 2    | 如果待删除节点为红色且只有一个孩子或者没有孩子               | 直接删除待删除节点                                           |
| 3    | 如果待删除节点是黑色，唯一子节点一定是红色                   | 将待删除节点删除，并将子节点颜色改变成黑色                   |
| 4    | 如果待删除节点是黑色，没有孩子^[1]^，且待删除节点的兄弟节点为红色^[2]^ | 将待删除节点的父亲节点左旋，使得兄弟节点处于原来父亲的位置，并将原来的父亲节点变为红色，原来的兄弟节点变为黑色，此时会发现待删除节点的现在兄弟节点的颜色为黑色^[3]^，即转化为情况5^[4]^ |
| 5    | 如果待删除节点是黑色，没有孩子，且待删除节点的兄弟节点为黑色，父亲节点为红色 | 删除待删除节点，将其兄弟节点调整为红色，之后进行旋转操作，使原来的父节点作为原来的兄弟节点的孩子 |
|      | 如果待删除节点是黑色，没有孩子，且待删除节点的兄弟节点为黑色，父亲节点为黑色 | 将待删除节点删去后，将其兄弟节点染为红色，然后递归检查其祖先节点的情况，如果有需要修正的则按照各个情况进行修正 |
|      |                                                              |                                                              |

> ^[1]^ 因为待删除节点最多只有一个孩子，因此如果它自己为黑色，则黑高为1，此时如果具有有孩子，则只能为红色，如果为黑色则黑高不相等，不符合红黑树定义
>
> ^[2]^ 兄弟为红色意味着他们的父亲只能为黑色
>
> ^[3]^ 此时待删除节点的兄弟节点为其原兄弟节点的孩子
>
> ^[4]^ 操作的目的是将待删除节点的兄弟节点转变为黑色

### 红黑树与AVL树的区别

AVL树是严格平衡二叉树，要求每个节点的左右子树高度差不超过1，

红黑树条件更宽松一点，要求任何一条路径的长度不超过其他路径长度的两倍

AVL树的查找效率更高，平衡调整的成本更高

在频繁查找时应选用AVL树，在频繁插入删除时，应选用红黑树

 

## B+树

### B+树的结构

一棵M阶的B+树满足下列的结构属性：

1. 根节点要么是叶子，要么有2~M个孩子

2. 所有的非叶节点(除了根节点)有M/2(向上取整)到M个孩子

3. 所有的叶节点都是在相同深度的

### B+树的特点

B+树主要被应用于数据库的索引结构，B+树**只有叶节点存放数据**，其余节点只存放索引

### B+树的添加

**遵循分裂策略**

最初当B+树是一棵空树时，根节点作为第一个叶子节点，在根节点中添加键值即可

当根节点中键值达到了M+1时将其等分(一般将较少的部分拆分到左边)，将右子树的最小值作为键值，按照按照此策略依次进行后续添加

### B+树的删除

**遵循合并原则**

当B+树删除叶子中的一个值时，如果该叶子中还有不少于2个值，则可无需进行任何操作

当删除后叶子只剩一个值，则将其合并至相邻叶子节点，然后按照添加规则进行修改，如果某一个节点只剩一个叶节点，则将该叶节点合并到相邻节点，并取消该节点的父亲

### 2-3树易错点整理

- 当只有root一个节点时，root没有孩子

- B+树的深度和查找的时间复杂度

  - Depth(M, N) = O( ⌈ log~⌈M/2⌉~^N^ ⌉ )


  - T~Find~ (M, N) = O( log^N^ )


## 颠倒文件索引(Inverted File Index)

#### 术语文档关联矩阵（Term-Document Incidence Matrix）

- 实现方式


将文档依次编号，然后检索每一个属于是否出现在文档中，如果出现，则该矩阵对应值为1，如果未出现，则为0。如下表所示

|          | 1    | 2    | 3    | 4    |
| -------- | ---- | ---- | ---- | ---- |
| a        | 0    | 1    | 1    | 1    |
| arrived  | 0    | 0    | 1    | 1    |
| damaged  | 0    | 1    | 0    | 0    |
| delivery | 0    | 0    | 1    | 0    |

#### 颠倒文件索引

颠倒文件索引是术语文档关联矩阵的紧凑版本

- 索引(Index)

索引是一种在文本中定位给定术语的机制（mechanism）

- 倒排文件(Inverted file)

倒排文件包含一个指向所有出现过对应术语的文本的指针列表

倒排文件也叫倒排索引，是一种利用属性值来确定记录位置的方式

- 实现方式


将术语编号，并将术语作为关键词，记录下出现过该术语的文档和术语在所有文档中出现的次数（一个文档最多记一次次数），如下表所示

| No.  | Term     | Times; Documents |
| ---- | -------- | ---------------- |
| 1    | a        | <3; 2,3,4>       |
| 2    | arrived  | <2; 3,4>         |
| 3    | damaged  | <1; 2>           |
| 4    | delivery | <1; 3>           |
| 5    | fire     | <1; 2>           |

在用户输入关键词后，取所有关键词的文档交集，返回的文档编号即符合要求的文档

- 优化方式


对于该方式，由于是取所有关键词的文档交集，可以采用优先检索出现频率少（即包含该关键词的文档数量少的关键词）的关键词的方法，可以有效增加检索速度

- 索引生成算法


```
while ( read a document D ) {
  while ( read a term T in D ) {
	if ( Find( Dictionary, T ) == false )
		Insert( Dictionary, T );
		Get T’s posting list;
		Insert a node to T’s posting list;
  }
}
Write the inverted index to disk;
```

> 读取术语后需要进行的操作
>
> - 词干提取(word stemming)
>
> 对一个单词进行处理，使其还原成词根的形式
>
> 具体操作
>
> 1. 将术语拆分成若干个单词
>
> 2. 处理每个单词，使其只剩下词干或词根形式
>
> - 寻找停止词（stop word）
>
>
> 有些词非常常见，几乎每个文档都包含它们，例如“a”和“it”。为它们编制索引是无用的。它们被称为停止语。我们可以从原始文件中删除它们。

- 存储索引的方式

```
BlockCnt = 0; 
while ( read a document D ) {
	while ( read a term T in D ) {
		if ( out of memory ) {
			Write BlockIndex[BlockCnt] to disk;
			BlockCnt ++;
			FreeMemory;
		}
    if ( Find( Dictionary, T ) == false )
    	Insert( Dictionary, T );
   	Get T’s posting list;
   	Insert a node to T’s posting list;
 }
}
```

#### 分布式索引

##### 术语分割索引（Term-partitioned index）

将所有的术语按照首字母存储在不同的服务器上，需要检索的时候则先检索每一台服务器上的单词，然后进行归并运算

##### 文档分割索引（Document-partitioned index）

将文档按照一定的数量分别存放在各个服务器上，然后检索的时候搜索对应的服务器即可。即将文件号为一个范围内的文件存在一个节点上

#### 动态索引

##### 实现方式

随着时间的推移，文档逐渐加入，之后对加入的文档进行如下操作

1. 对词典中已有的术语进行更新

2. 加入原词典中未有的术语

> 如果文档被删除则将该文档做标记
>

##### 优化方式

1. 可以利用主索引和辅助索引（auxiliary index）配合检索，主索引负责存储原有的索引，数据较难更改，辅助索引负责查找新的文档
2. 关键词压缩
   - 将所有关键词去除停止词后压缩成一串字符串进行查找


3. 优化倒排列表（Posting List）
   - 即按照各个文档编号之间的关系存储文档编号，而非直接存储编号，这样避免了文档编号过大所带来的的空间开销


4. 设置搜索引擎的搜索阈值（Thresholding）
   1. 对文档设置阈值
      - 即仅仅检索并返回关联度最高的x篇文档
      - 缺点
        - 由于截断，我们可能会错过一些相关文件
        - 对于Boolean queries不可行
   2. 对查询设置阈值
      - 按频率升序对查询词进行排序或只根据原始查询词的某个百分比进行搜索

### 搜索引擎的核心要素

1. 索引速度
   - 即每小时能够检索的文档数

2. 搜索速度
   - 延迟是索引大小的函数

3. 查询语言的表达能力
   - 即能够表达复杂的信息需求或加快复杂查询的速度

4. 用户幸福感
   1. 数据检索(data retrieval)性能评估依据
      1. 响应时间
      2. 索引空间
      2. 注意：数据检索性能评估要在确定性能之后才能进行
   2. 信息检索(information retrieval)绩效评估依据
      1. 答案集的相关性

> 注意data retrieval和information retrieval的区别，会考
>

### 衡量关联性（relevance）的要素

1. 基准文档集

2. 一组基准查询

3. 对每个查询文档对进行相关或不相关的二元评估

|               | Relevant | Irrelevant |
| ------------- | -------- | ---------- |
| Retrieved     | R~R~     | I~R~       |
| Not Retrieved | R~N~     | I~N~       |

Precision(查准率) P = R~R~ / (R~R~ + I~R~)

Recall(查全率)    R = R~R~ / (R~R~ + R~N~)

> precision和recall一般成负相关
>

## 最左堆（leftist heaps）

### 基本概念

- 目标
  - 在O(N)的时间复杂度内加速归并

- NPL(null path length)

  - Npl(X)指节点X到任意叶节点的最短路径

  - 注意：Npl(NULL)=-1

  - Npl(X) = min { Npl(C) + 1 for all C as children of X }

-  最左堆定义
  - 最左堆定义为在堆中的每个节点X的左子树的Npl不小于右子树的Npl

### 最左堆相关定理

- 定理
  - 右路径上具有r个节点的最左树至少有2^r^-1个节点（可用归纳法证明）

- 最左堆的好处
  - 我们能在右路径上进行所有的工作，从而使其时间复杂度降低

### 最左堆实现步骤

##### 归并操作递归版本

1. 基本操作
   1. Merge( H1->Right, H2 )
      - 此处以H1>H2为例
      - 注意，此处归并并非只做一次，而是递归归并
   2. Attach( H2, H1->Right )
   3. Swap(H1->Right, H1->Left ) if necessary


![image-20220417222232828](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220417222232828.png)

![image-20220417222243767](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220417222243767.png)

2. 伪代码：

```
PriorityQueue  Merge ( PriorityQueue H1, PriorityQueue H2 )
{ 
	if ( H1 == NULL )   return H2;	
	if ( H2 == NULL )   return H1;	
	if ( H1->Element < H2->Element )  return Merge1( H1, H2 );
	else return Merge1( H2, H1 );
}
static PriorityQueue
Merge1( PriorityQueue H1, PriorityQueue H2 )
{ 
	if ( H1->Left == NULL ) 	/* single node */
		H1->Left = H2;	/* H1->Right is already NULL 
				    and H1->Npl is already 0 */
	else {
		H1->Right = Merge( H1->Right, H2 );     /* Step 1 & 2 */
		if ( H1->Left->Npl < H1->Right->Npl )
			SwapChildren( H1 );	/* Step 3 */
		H1->Npl = H1->Right->Npl + 1;
	} /* end else */
	return H1;
}
```

##### 归并操作迭代版本

- 基本操作
  1. Sort the right paths without changing their left children
  2. Swap children if necessary

##### 删除根节点操作

1. Delete the root
2. Merge the two subtrees

### 时间复杂度

合并两个左堆的时间复杂度为O(log^N^)，相当于把右路径的长度相加

删除根节点的时间复杂度为O(log^N^)

> 注意：迭代版本和递归版本的归并操作时间复杂度均相同，迭代版本效率高一点
>

## 斜堆（skew heaps）

### skew heaps特点

斜堆是最左堆的一个简化版本，斜堆不需要存储Npl信息

斜堆在合并的时候**始终交换**左右子节点，**除非右路径上最大的节点没有交换其子节点**

### skew heaps定义

1. 只有一个元素的堆是斜堆
2. 两个斜堆通过斜堆的合并操作，得到的结果仍是斜堆

### 合并斜堆

> 注意：插入斜堆是合并斜堆的一个特例，因此不独立进行分析

#### 递归实现

1. 如果一个空左倾堆和一个非空左倾堆合并，返回非空左倾堆

2. 如果两个左倾堆都非空，那么比较两个根节点，取较小的节点为新的根节点，将较大堆的根节点作为其右孩子

3. 交换根节点的右子堆和左子堆
4. 之后递归比较原较小根节点堆的右子堆和原较大根节点堆，重复上方步骤递归合并

以下为合并H1和H2的结果图

![image-20220415185141471](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415185141471.png)

#### 迭代实现

1. 分别把两个堆的右路径上所有节点的右子树分离出来。

2. 把分离出来的子树按根节点元素升序（广义上的升序）排列。

3. 把倒数第二个树左右子树交换，把最后一个树作为倒数第二个树的左子树，之后重复此操作依次向前直到最小根元素为止

### 时间复杂度

merge操作的时间复杂度：摊还时间为O(logN)，最坏情况为O(N)

### skew heaps注意点

1. 倾斜堆的优点是无需额外的空间来保持路径长度，也无需测试来确定何时交换子级

2. 精确确定左堆和斜堆的预期右路径长度是一个开放的问题。

3. 如果将节点1到2^k^-1插入一个空的最小歪堆，则所得到的树始终是一个满二叉树

##  二项队列

### 二项队列介绍

二项队列不是一棵树，它是一个森林，由一组堆树按照一定顺序组成的森林

### 二项队列的性质

1. 每一颗树都是一个有约束的堆序树，叫做二项树
2. 高度为k的第k个二项树Bk由一个根节点和B~0~, B~1~, .......B~k-1~构成
3. 高度为k的二项树的结点个数为2^k^

我们可以用二项树的结合表示任意大小的优先队列。例如，大小为13的优先队列就可以用B~3~，B~2~，B~0~来表示，二进制的表示为1101

- 二项队列实例

![image-20220417225239463](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220417225239463.png)

### 二项队列定理

一个有着N个元素的二项队列可以通过N次成功的插入操作从而在O(N)的时间内生成

### 二项队列的操作

#### 查找最小值

二项队列的最小值为某一个二项树的根

在这里最多有<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415185954535.png" alt="image-20220415185954535" style="zoom:50%;" />个根

因此查找的时间复杂度为 O(log^N^)

#### 归并操作

从B~0~开始进行归并，较简单，此处不介绍

时间复杂度为 O(log^N^)

#### 插入操作

插入操作是归并操作的特殊情况，略

因为插入N个值的最差情况时O(N)，因此插入操作的均摊时间复杂度为O(1)

#### 删除最小值操作

1. 进行一次查找最小值操作，令最小值在B~k~上
   - 需要O(log N)时间
2. 将B~k~暂时从二项森林 H 中去除，令剩余的二项树的森林为 H’
   - 需要O(1)时间
3. 从B~k~中将根去除，令得到的二项树的森林为H’‘
   - 需要O(log N)时间
4. 归并H‘和H’‘
   - 需要O(log N)时间

### 二项队列代码结构

```c
typedef struct BinNode *Position;
typedef struct Collection *BinQueue;
typedef struct BinNode *BinTree;  /* missing from p.176 */

struct BinNode 
{ 
	ElementType	    Element;
	Position	    LeftChild;
	Position 	    NextSibling;
} ;

struct Collection 
{ 
	int	    	CurrentSize;  /* total number of nodes */
	BinTree	TheTrees[ MaxTrees ];
} ;

BinTree CombineTrees( BinTree T1, BinTree T2 )
{  /* merge equal-sized T1 and T2 */
	if ( T1->Element > T2->Element )
		/* attach the larger one to the smaller one */
		return CombineTrees( T2, T1 );
	/* insert T2 to the front of the children list of T1 */
	T2->NextSibling = T1->LeftChild;
	T1->LeftChild = T2;
	return T1;
}

BinQueue  Merge( BinQueue H1, BinQueue H2 )
{	BinTree T1, T2, Carry = NULL; 	
	int i, j;
	if ( H1->CurrentSize + H2-> CurrentSize > Capacity )  ErrorMessage();
	H1->CurrentSize += H2-> CurrentSize;
	for ( i=0, j=1; j<= H1->CurrentSize; i++, j*=2 ) {
	    T1 = H1->TheTrees[i]; T2 = H2->TheTrees[i]; /*current trees */
	    switch( 4*!!Carry + 2*!!T2 + !!T1 ) { 
		case 0: /* 000 */
	 	case 1: /* 001 */  break;	
		case 2: /* 010 */  H1->TheTrees[i] = T2; H2->TheTrees[i] = NULL; break;
		case 4: /* 100 */  H1->TheTrees[i] = Carry; Carry = NULL; break;
		case 3: /* 011 */  Carry = CombineTrees( T1, T2 );
			            H1->TheTrees[i] = H2->TheTrees[i] = NULL; break;
		case 5: /* 101 */  Carry = CombineTrees( T1, Carry );
			            H1->TheTrees[i] = NULL; break;
		case 6: /* 110 */  Carry = CombineTrees( T2, Carry );
			            H2->TheTrees[i] = NULL; break;
		case 7: /* 111 */  H1->TheTrees[i] = Carry; 
			            Carry = CombineTrees( T1, T2 ); 
			            H2->TheTrees[i] = NULL; break;
	    } /* end switch */
	} /* end for-loop */
	return H1;
}
ElementType  DeleteMin( BinQueue H )
{	BinQueue DeletedQueue; 
	Position DeletedTree, OldRoot;
	ElementType MinItem = Infinity;  /* the minimum item to be returned */	
	int i, j, MinTree; /* MinTree is the index of the tree with the minimum item */

	if ( IsEmpty( H ) )  {  PrintErrorMessage();  return –Infinity; }

	for ( i = 0; i < MaxTrees; i++) {  /* Step 1: find the minimum item */
	    if( H->TheTrees[i] && H->TheTrees[i]->Element < MinItem ) { 
		MinItem = H->TheTrees[i]->Element;  MinTree = i;    } /* end if */
	} /* end for-i-loop */
	DeletedTree = H->TheTrees[ MinTree ];  
	H->TheTrees[ MinTree ] = NULL;   /* Step 2: remove the MinTree from H => H’ */ 
	OldRoot = DeletedTree;   /* Step 3.1: remove the root */ 
	DeletedTree = DeletedTree->LeftChild;   free(OldRoot);
	DeletedQueue = Initialize();   /* Step 3.2: create H” */ 
	DeletedQueue->CurrentSize = ( 1<<MinTree ) – 1;  /* 2MinTree – 1 */
	for ( j = MinTree – 1; j >= 0; j – – ) {  
	    DeletedQueue->TheTrees[j] = DeletedTree;
	    DeletedTree = DeletedTree->NextSibling;
	    DeletedQueue->TheTrees[j]->NextSibling = NULL;
	} /* end for-j-loop */
	H->CurrentSize  – = DeletedQueue->CurrentSize + 1;
	H = Merge( H, DeletedQueue ); /* Step 4: merge H’ and H” */ 
	return MinItem;
}
```

## 回溯算法(Backtracking)

回溯算法本质上就是dfs+剪枝(pruning)

主要难点在于剪枝，具体说明略

#### α pruning

![image-20220417231526206](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220417231526206.png)

> 即爷节点求最大值，子节点求最小值时，如果右孩子的任意孩子中已经有节点小于左孩子的最小值，则直接进行剪枝

#### β pruning

![image-20220417231710434](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220417231710434.png)

#### α-β pruning

即两种剪枝的结合

## 分治算法(Divide and Conquer)

分支算法就是将一个问题递归调地分解成若干个子问题，然后对于子问题进行求解，最后依次合并求解的结果得到最优解

朴素的递归式：T(N) = aT(N/b) + f(N)

# 递归时间复杂度分析

## 递归树法

递归树法主要用于求解递归的时间复杂度问题

递归树本质就是将所有的递归情况转化成一棵树，针对该树的每一层计算所需的时间复杂度，最后将所有层的时间复杂度相加即可

> 注意：递归树所求的时间复杂度可以是一个范围，但是数量级要相同

## 主定理(Master Theorem)

主定理适用于求解如下递归式算法的时间复杂度：

其中：

- n 是问题规模大小；
- a 是原问题的子问题个数；
- n/b 是每个子问题的大小，这里假设每个子问题有相同的规模大小；
- f(n) 是将原问题分解成子问题和将子问题的解合并成原问题的解的时间。
- 对上面的式子进行分析，得到三种情况：

![在这里插入图片描述](https://img-blog.csdnimg.cn/c16a676ee61546e6a032acee7a5658f4.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU2MDkxMw==,size_16,color_FFFFFF,t_70#pic_center)

- 其他形式的主定理

![image-20220415220848287](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220415220848287.png)
