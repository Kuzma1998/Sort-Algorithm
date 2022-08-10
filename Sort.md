```c++
#include <bits/stdc++.h>
using namespace std;

void bubbleSort(vector<int> &arr);
void insertSort(vector<int> &arr);
void selectSort(vector<int> &arr);
void shellSort(vector<int> &arr);
void quickSort(vector<int> &arr);
void heapSort(vector<int> &arr);
void mergeSort(vector<int> &arr);

/*
    比较相邻的元素，若前一个小于后一个，交换位置，这样每次就选出最小的，逐渐较少比较的次数即可
*/
void bubbleSort(vector<int> &arr)
{
    int n = arr.size();
    for (int i = n - 1; i >= 0; --i)
    { // 比较轮数
        int flag = 0;
        for (int j = 0; j < i; ++j)
        { // 每一轮的比较
            if (arr[j] > arr[j + 1])
            {
                swap(arr[j], arr[j + 1]);
                flag = 1;
            }
        }
        if (!flag)
            break;
    }
}

/*
    插入排序，每趟将一个待排序的关键字按照其值大小插入到已经排好序的部分序列的适当位置
*/
void insertSort(vector<int> &arr)
{
    int n = arr.size();
    for (int i = 1; i < n; ++i)
    {
        int temp = arr[i];                      // 待插入元素
        int j = i;                              // 待插入坐标
        for (; j > 0 && temp < arr[j - 1]; --j) //如果待插入的元素小于当前的元素前一个，则把当前元素前一个往后移动，给待插入的元素腾位置
        {
            arr[j] = arr[j - 1];
        }
        arr[j] = temp; // temp大于当前元素前一个 找到了插入位置，也就是当前的j
    }
}

/*
    首先在未排序的序列找到最小or最大的元素坐标，然后放到排序序列的起始位置，然后再继续从未排序的地方查找
*/
void selectSort(vector<int> &arr)
{
    int n = arr.size();
    for (int i = 0; i < n; ++i)
    {
        int minIndex = i;
        for (int j = i + 1; j < n; ++j)
        {
            if (arr[j] < arr[minIndex])
            {
                minIndex = j;
            }
        }
        swap(arr[i], arr[minIndex]); // 把最小的放到i这个位置
    }
}

/*
    缩小增量的选择排序
*/
void shellSort(vector<int> &arr)
{
    int n = arr.size();
    // 缩小增量
    for (int d = n / 2; d > 0; d = d / 2)
    {
        // 插入排序
        for (int i = d; i < n; ++i)
        {
            int temp = arr[i];
            int j = i;
            for (; j > 0 && arr[j - d] > temp; j -= d)
            {
                arr[j] = arr[j - d];
            }
            swap(arr[j], temp);
        }
    }
}

/*
    选择主元,把主元放到合适的位置
*/
void quickSort(vector<int> &arr, int l, int r)
{
    if (l < r)
    {
        int temp = arr[l];
        int i = l + 1;
        int j = r;
        while (i <= j)
        {
            while (i <= j && arr[j] > temp)
            {
                --j;
            }
            while (i <= j && arr[i] <= temp)
            {
                ++i;
            }
            if (i <= j)
                swap(arr[i], arr[j]);
            else
                break;
        }
        swap(arr[j], arr[l]);
        quickSort(arr, l, j - 1);
        quickSort(arr, j + 1, r);
    }
}

/*
    调节节点位置  建立大根堆
    @param i: 需要调节的节点
    @param N: 堆的大小
*/
void precDown(vector<int> &arr, int i, int N)
{
    int temp = arr[i];
    int parent;
    int child;
    for (parent = i; parent * 2 + 1 < N; parent = child)
    {
        // 取当前父节点的子节点里面的较大值
        child = parent * 2 + 1;
        if (child != N - 1 && arr[child + 1] > arr[child])
            ++child;
        // 若要调节的节点的值大于当前父节点的子节点的值,跳出 说明 temp要在的位置就是此时的父节点
        if (temp > arr[child])
            break;
        // 若要调节的节点的值小于当前父节点的孩子节点,说明要继续往下找,找之前先把子节点的值上浮
        else
            arr[parent] = arr[child];
    }
    arr[parent] = temp;
}

/*
    堆排序
 */
void heapSort(vector<int> &arr)
{
    int n = arr.size();
    // 建立大堆
    for (int i = n / 2 - 1; i >= 0; --i)
    {
        precDown(arr, i, n);
    }

    for (int i = n - 1; i > 0; --i)
    {
        // 交换元素,最大的放后面
        swap(arr[i], arr[0]);
        // 调节0这号元素,把最大的浮上来
        precDown(arr, 0, i);
    }
}



//交换
void merge(vector<int> &array, vector<int> &temp, int leftStart, int rightStart, int rightEnd)
{
    int leftEnd = rightStart - 1; // 左边的结束
    int tmp = leftStart; // 用于临时数组增长的索引
    int exchangeLength = rightEnd - leftStart + 1; // 此次交换的长度
    // 排序的逻辑
    while (leftStart <= leftEnd && rightStart <= rightEnd)
    {
        if (array[leftStart] < array[rightStart])
        {
            temp[tmp++] = array[leftStart++];
        }
        else
        {
            temp[tmp++] = array[rightStart++];
        }
    }
    while (leftStart <= leftEnd)
    {
        temp[tmp++] = array[leftStart++];
    }
    while (rightStart <= rightEnd)
    {
        temp[tmp++] = array[rightStart++];
    }
    
    // 写回原数组
    while (exchangeLength>0)
    {   exchangeLength--;
        array[rightEnd] = temp[rightEnd];
        --rightEnd;
    }
}


void mSort(vector<int> &arr, vector<int> &temp, int left, int right)
{

    // 递归到左右都只有一个节点返回
    if (left < right)
    {   
        
        int center = (left + right) >> 1;
        mSort(arr, temp, left, center); // 排序左边
        mSort(arr, temp, center + 1, right); // 排序右边
        merge(arr, temp, left, center + 1, right); // 合并
    }
}

void mergeSort(vector<int> &arr)
{   // 用于交换的临时数组
    vector<int> temp = vector<int>(arr.size(), 0);
    // 递归排序
    mSort(arr, temp, 0, arr.size() - 1);
}



int main()
{
    vector<int> arr = {3, 2, 5, 1, 4, 6, 7, 24, 44, 11, 66, 43};
    // bubbleSort(arr);
    // insertSort(arr);
    // selectSort(arr);
    // shellSort(arr);
    // quickSort(arr, 0, arr.size() - 1);
    // heapSort(arr);
    // 默认大顶堆,传入greater<int> 为小顶堆
    // priority_queue<int,vector<int>,greater<int>> queue;
    // for(auto elem:arr){
    //     queue.push(elem);
    // }
    // for(int i=0;i<arr.size();++i){
    //     arr[i] = queue.top();
    //     queue.pop();
    // }

    mergeSort(arr);

    for (auto elem : arr)
    {
        cout << elem << " ";
    }
    return 0;
}
```
