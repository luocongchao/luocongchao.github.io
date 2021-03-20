# 排序算法



## 1、归并排序（Merge Sort）

归并排序，是创建在归并操作上的一种有效的排序算法。算法是采用分治法（Divide and Conquer）的一个非常典型的应用，且各层分治递归可以同时进行。归并排序思路简单，速度仅次于快速排序，为稳定排序算法，一般用于对总体无序，但是各子项相对有序的数列。

归并排序是用分治思想，分治模式在每一层递归上有三个步骤：

- 分解（Divide）：将n个元素分成个含n/2个元素的子序列。
- 解决（Conquer）：用合并排序法对两个子序列递归的排序。
- 合并（Combine）：合并两个已排序的子序列已得到排序结果。

![sorting2](https://raw.githubusercontent.com/luocongchao/luocongchao.github.io/master/imgs/sorting2.gif)

上图中首先把一个未排序的序列从中间分割成2部分，再把2部分分成4部分，依次分割下去，直到分割成一个一个的数据，再把这些数据两两归并到一起，使之有序，不停的归并，最后成为一个排好序的序列。

#### 1）时间复杂度

> 平均时间复杂度：O(nlogn)
> 最佳时间复杂度：O(n)
> 最差时间复杂度：O(nlogn)
> 空间复杂度：O(n)
> 排序方式：In-place
> 稳定性：稳定

不管元素在什么情况下都要做这些步骤，所以花销的时间是不变的，所以该算法的最优时间复杂度和最差时间复杂度及平均时间复杂度都是一样的为：O( nlogn )

归并的空间复杂度就是那个临时的数组和递归时压入栈的数据占用的空间：n + logn；所以空间复杂度为: O(n)。

归并排序算法中，归并最后到底都是相邻元素之间的比较交换，并不会发生相同元素的相对位置发生变化，故是稳定性算法。

#### 2）C#代码实现

```c#
/// <summary>
/// 归并排序
/// 递归均匀分割数据一直分割直到数组内只剩一个元素
/// 分割完后把相邻的两个数组递归合并直到合并成一个数组
/// </summary>
/// <param name="list"></param>
/// <returns></returns>
public static int[] SortMerge(this int[] list) {
    // 备份分割好的左边数组
    leftArray = new int[list.Length >> 1];
    list.SplitSort(0, list.Length);
    leftArray = null;
    return list;
}
static int[] leftArray;
/// <summary>
/// 数组递归分割和合并
/// </summary>
/// <param name="list"></param>
/// <param name="begin">数组起始索引</param>
/// <param name="end">数组结束索引</param>
static void SplitSort(this int[] list, int begin, int end) {
    if (end - begin < 2) return;
    int mid = (end + begin) >> 1;
    list.SplitSort(begin, mid);
    list.SplitSort(mid, end);
    list.MergeSort(begin, mid, end);
}
/// <summary>
/// 数据合并
/// </summary>
/// <param name="list"></param>
/// <param name="begin">数组起始索引</param>
/// <param name="mid">数组中间索引</param>
/// <param name="end">数组结束索引</param>
static void MergeSort(this int[] list, int begin, int mid, int end) {
    // 左边数组开始索引  左边数组结束索引
    int li = 0, le = mid - begin;
    // 右边数组开始索引  右边数组结束索引
    int ri = mid, re = end;
    // 下次应该赋值的数组索引
    int ai = begin;
    // 备份数组左边的部分
    for (int i = li; i < le; i++) {
        leftArray[i] = list[begin + i];
    }
    // 左边数组合并完成代表两个数组已经合并完毕
    while (li < le) {
        // 右边索引对应的元素小就赋值右边的元素 
        if (ri < re && list[ri] < leftArray[li]) {
            list[ai++] = list[ri++];
        }
        // 左边索引对应的元素小就赋值左边的元素 
        else {
            list[ai++] = leftArray[li++];
        }
    }
}
```



------



## 2、快速排序（Quick Sort）

快速排序，又称划分交换排序（partition-exchange sort）

通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

快速排序使用分治法（Divide and conquer）策略来把一个序列（list）分为两个子序列（sub-lists）。

> ① 从数列中挑出一个元素，称为 “基准”（pivot），
> ② 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作。
> ③ 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

递归到最底部时，数列的大小是零或一，也就是已经排序好了。这个算法一定会结束，因为在每次的迭代（iteration）中，它至少会把一个元素摆到它最后的位置去。

![sorting2](https://raw.githubusercontent.com/luocongchao/luocongchao.github.io/master/imgs/sorting1.gif)

#### 1）复杂度

> 平均时间复杂度：O(NlogN)
> 最佳时间复杂度：O(NlogN)
> 最差时间复杂度：O(N^2)
> 空间复杂度：根据实现方式的不同而不同

#### 2）C#代码实现

```C#
/// <summary>
/// 快速排序
/// 递归获取轴点元素的过程 在获得轴点元素后把小于轴点元素的放左边  大于轴点元素的放右边
/// </summary>
/// <param name="list"></param>
/// <returns></returns>
public static int[] SortQuick(this int[] list) {
    list.Quick(0, list.Length);
    return list;
}
/// <summary>
/// 递归获取轴点元素索引
/// </summary>
/// <param name="list"></param>
/// <param name="begin"></param>
/// <param name="end"></param>
static void Quick(this int[] list, int begin, int end) {
    if (end - begin < 2) return;
    int mid = list.GetPiovtIndex(begin, end);
    list.Quick(begin, mid);
    list.Quick(mid + 1, end);
}
/// <summary>
/// 获取轴点元素索引并把小于轴点元素的放轴点左边  大于轴点元素的放轴点右边
/// </summary>
/// <param name="list">数组</param>
/// <param name="begin">数组区间起始索引</param>
/// <param name="end">数组区间结束索引</param>
/// <returns></returns>
static int GetPiovtIndex(this int[] list, int begin, int end) {
    // 随机获取轴点元素索引
    var swapIndex = begin + (int)new Random().NextDouble() * (end - begin);
    // 轴点元素和begin元素互换位置 规避最差时间复杂度
    var pivot = list[swapIndex];
    list[swapIndex] = list[begin];
    list[begin] = pivot;
    end--
    // 开始索引等于结束索引代表已经完成排序
    while (begin < end) {
        while (begin < end) {
            // 右边的数据大于轴点数据  那么右边的索引--
            if (pivot < list[end]) end--;
            else {
                // 否则把右边的数据移动到左边  并且左边的索引++
                list[begin++] = list[end];
                // 轮到左边进行比较
                break;
            }
        }
        while (begin < end) {
            // 左边的数据小于轴点数据  那么左边的索引++
            if (pivot > list[begin]) begin++;
            else {
                // 否则把左边的数据移动到右边  并且右边的索引--
                list[end--] = list[begin];
                // 轮到右边进行比较
                break;
            }
        }
    }
    list[begin] = pivot;
    return begin;
}
```



[来源：https://www.byteflying.com/archives/6171](https://www.byteflying.com/archives/6171)

