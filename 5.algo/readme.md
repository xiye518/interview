# 5.算法

算法与数据结构，从链表到堆栈、队列、各种树（二叉树、二叉查找树、AVL树、B树、B+树、红黑树），八大排序算法，分治法、动态规划算法、贪心算法、回朔法、、kmp算法、快慢指针，使用map空间换时间等；

* [如何在最短的时间内搞定数据结构和算法，应付面试?](https://www.zhihu.com/question/28580777/answer/530047115)
* [记一次手撕算法面试：字节跳动的面试官把我四连击了](https://juejin.im/post/5ddfa3def265da05ef59fe6e)
* ringbuffer 的两个 Go 实现：
    * https://github.com/smallnest/ringbuffer
    * https://github.com/Allenxuxu/ringbuffer
* [漫画算法：如何判断链表有环？](https://mp.weixin.qq.com/s?__biz=MzIxMjE5MTE1Nw==&mid=2653189798&idx=1&sn=c35c259d0a4a26a2ee6205ad90d0b2e1&chksm=8c99047cbbee8d6a452fbb171133551553a825c83fb8b0cc66210dcda842c61157a07baaeb6b&scene=21#wechat_redirect)
* [漫画：什么是B+树？](https://zhuanlan.zhihu.com/p/54102723)
* [如何给100亿个数字排序?](https://blog.csdn.net/nigelyq/article/details/52766879)
* [10亿个字符串的排序问题](https://blog.csdn.net/woshifeixingzhuiyue/article/details/41946339)
* 云栖社区 [亿万级数据处理的高效解决方案](https://yq.aliyun.com/articles/643124)
* [七分钟理解什么是 KMP 算法](https://zhuanlan.zhihu.com/p/76348091)
* [漫画：什么是字符串匹配算法？](https://zhuanlan.zhihu.com/p/107507971)
* 八大排序算法 [我说我不会算法，阿里把我挂了。](https://blog.csdn.net/Java_3y/article/details/104897426?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
* [31道二叉树算法，给自己的五一礼物](https://juejin.im/post/5ead8119e51d454dc146626e)
* []()
* []()
* []()

###  什么是数据结构？
简单说，数据结构就是一个容器，以某种特定的布局存储数据。这个“布局”使得数据结构在某些操作上非常高效，在另一些操作上则不那么高效。你的目标就是理解数据结构，这样就能为手头的问题选择最优的数据结构。

### 为什么我们需要数据结构？
由于数据结构用来以有组织的形式存储数据，而且数据是计算机科学中最重要的实体，因此数据结构的真正价值显而易见。无论你解决什么问题，你都必须以这种或那种方式处理数据——员工的工资，股票价格，购物清单，甚至简单的电话簿等等。根据不同的场景，数据需要以特定格式存储。目前有一些数据结构可以满足我们以不同格式存储数据的需求。

### 常用的数据结构
我们首先列出最常用的数据结构，然后再按个讲解：
- 数组
- 堆
- 栈
- 队列
- 链表
- 树
- 图
- 字典树（Tries，这是一种高效的树，有必要单独列出来）
- 哈希表


## 1.堆排、快排，常见的八种排序及其复杂度
## 2.一百亿条数据如何排序
#### 场景
 要给100亿个数字排序，100亿个 int 型数字放在文件里面大概有 37.2GB，非常大，内存一次装不下了。那么肯定是要拆分成小的文件一个一个来处理，最终在合并成一个排好序的大文件。

#### 实现思路
1.把这个37GB的大文件，用哈希分成1000个小文件，每个小文件平均38MB左右（理想情况），把100亿个数字对1000取模，模出来的结果在0到999之间，每个结果对应一个文件，所以我这里取的哈希函数是 h = x % 1000，哈希函数取得"好"，能使冲突减小，结果分布均匀。

2.拆分完了之后，得到一些几十MB的小文件，那么就可以放进内存里排序了，可以用快速排序，归并排序，堆排序等等。

3.1000个小文件内部排好序之后，就要把这些内部有序的小文件，合并成一个大的文件，可以用二叉堆来做1000路合并的操作，每个小文件是一路，合并后的大文件仍然有序。

首先遍历1000个文件，每个文件里面取第一个数字，组成 (数字, 文件号) 这样的组合加入到堆里（假设是从小到大排序，用小顶堆），遍历完后堆里有1000个 (数字，文件号) 这样的元素
然后不断从堆顶拿元素出来，每拿出一个元素，把它的文件号读取出来，然后去对应的文件里，加一个元素进入堆，直到那个文件被读取完。拿出来的元素当然追加到最终结果的文件里。
按照上面的操作，直到堆被取空了，此时最终结果文件里的全部数字就是有序的了。
#### 思维拓展
类似的100亿个数字求和，求中位数，求平均数，套路就是一样的了。
求和：统计每个小文件的和，返回给master再求和就可以了。
求平均数：上面能求和了，再除以100亿就是平均数了
求中位数：在排序的基础上，遍历到中间的那个数就是中位数了。

## 二分查找算法