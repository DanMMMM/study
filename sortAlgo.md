### 插入排序  
平均时间复杂度*O(n^2)*，最坏时间复杂度*O(n^2)*，最好时间复杂度*O(n)*，空间复杂度*O(1)*，稳定算法。  
插入排序通过构建有序序列，对未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。  
```
function inertionSort(arr){
    var len = arr.length;
    var preIndex, current;
    for(let i = 1;i < len; i++){
        preIndex = i - 1;
        current = arr[i];
        while(preIndex >= 0 && arr[preIndex] > current){
            arr[preIndex + 1] = arr[preIndex];
            preIndex--;
        }
        arr[preIndex + 1] = current;
    }
    return arr;
}
``` 
9 7 8 2 5 1 3 6 4  
7 9 8 2 5 1 3 6 4  
7 8 9 2 5 1 3 6 4  
2 7 8 9 5 1 3 6 4  
2 5 7 8 9 1 3 6 4  
1 2 5 7 8 9 3 6 4  
1 2 3 5 7 8 9 6 4  
1 2 3 5 6 7 8 9 4  
1 2 3 4 5 6 7 8 9  
要点：已排序序列+未排序序列
### 希尔排序
平均时间复杂度*O(n^1.3)*，最坏时间复杂度*O(n^2)*，最好时间复杂度*O(n)*，空间复杂度*O(1)*，不稳定算法。  
将整个待排序序列分割成为若干自序列分别进行直接插入排序。
### 选择排序
平均时间复杂度*O(n^2)*，最坏时间复杂度*O(n^2)*，最好时间复杂度*O(n^2)*，空间复杂度*O(1)*，不稳定算法。  
先在未排序序列中找到最小元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小元素，然后放到已排序序列的末尾。以此类推，知道所有元素均排序完毕。 
```
function selectionSort(arr){
    var len = arr.length;
    var minIndex, temp;
    for(let i = 0;i < len;i++){
        minIndex = i;
        for(let j = i + 1;j < len;j++){
            if(arr[j] < arr[minIndex])
                minIndex = j;
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    return arr;
}
```
9 7 8 2 5 1 3 6 4  
1 7 8 2 5 9 3 6 4  
1 2 8 7 5 9 3 6 4  
1 2 3 7 5 9 8 6 4  
1 2 3 4 5 9 8 6 7  
1 2 3 4 5 6 8 9 7  
1 2 3 4 5 6 7 9 8  
1 2 3 4 5 6 7 8 9  
要点：已排序序列+未排序序列  
与插入排序不同的是：选择排序需要找到未排序序列中的最小值，将它放到已排序序列的末尾，而插入排序是寻找当前元素在已排序序列中的位置。
### 堆排序
平均时间复杂度*O(nlogn)*，最坏时间复杂度*O(nlogn)*，最好时间复杂度*O(nlogn)*，空间复杂度*O(1)*，不稳定算法。  
将待排序序列构建成大顶堆，将堆顶元素和最后一个元素交换，对当前堆调整为新堆，以此类推，达到有序区元素个数为n-1。
### 冒泡排序
平均时间复杂度*O(n^2)*，最坏时间复杂度*O(n^2)*，最好时间复杂度*O(n)*，空间复杂度*O(1)*，稳定算法。  
比较相邻元素，将小的元素浮到前面，知道没有再需要交换的元素。
```
function bubbleSort(arr){
    var len = arr.length;
    for(let i = 0;i < len - 1;i++){
        for(let j = 0;j < len -1;j++){
            if(arr[j] > arr[j+1]){
                let temp = arr[j+1];
                arr[j+1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
```
9 7 8 2 5 1 3 6 4  
7 8 2 5 1 3 6 4 9  
7 2 5 1 3 6 4 8 9  
2 5 1 3 6 4 7 8 9  
2 1 3 5 4 6 7 8 9  
1 2 3 4 5 6 7 8 9
### 快速排序
平均时间复杂度*O(nlogn)*，最坏时间复杂度*O(n^2)*，最好时间复杂度*O(nlogn)*，空间复杂度*O(nlogn)*，不稳定算法。  
从数列中挑出一个元素，称为‘基准’，重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准大的元素摆放在基准后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区操作；递归的把小于基准值元素的子数列和大于基准值的子数列排序。  
```
function quickSort(arr){
    if(arr.length <= 1) return arr;
    var flag = Math.floor(arr.length/2);
    var middle = arr.splice(flag, 1);
    var left = [];
    var right = [];
    for(let i = 0;i < arr.length;i++){
        if(arr[i] > middle){
            right.push(arr[i]);
        }else{
            left.push(arr[i]);
        }
    }
    return quickSort(left).concat(middle, quickSort(right));
}
```
9 7 8 2 5 1 3 6 4  
2 1 3 4 5 9 7 8 6  
2 1 3 4 5 7 6 8 9  
1 2 3 4 5 6 7 8 9  
要点：设立“基准值”，比基准小的放在左边，大的放在右边。
### 归并算法
平均时间复杂度*O(nlogn)*，最坏时间复杂度*O(nlogn)*，最好时间复杂度*O(nlogn)*，空间复杂度*O(n)*，稳定算法。  
分治法，将未排序序列分成n个子序列，对自序列分别采用归并算法，将排序后的子序列合并成最终的排序序列。  
### 计数排序
平均时间复杂度*O(n+k)*，最坏时间复杂度*O(n+k)*，最好时间复杂度*O(n+k)*，空间复杂度*O(n+k)*，稳定算法。  
统计数组中值为i的元素出现的次数，存入数组的第i项，对所有计数累加，反向填充目标数组。  
### 桶排序
平均时间复杂度*O(n+k)*，最坏时间复杂度*O(n^2)*，最好时间复杂度*O(n)*，空间复杂度*O(n+k)*，稳定算法。  
对每个元素按照最高位的大小放入大小为10的map中，在每个map进行排序。  
### 基数排序
平均时间复杂度*O(n *k)*，最坏时间复杂度*O(n *k)*，最好时间复杂度*O(n *k)*，空间复杂度*O(n+k)*，稳定算法。  
按照低位先排序，然后收集；再按照高位排序，然后再收集；以此类推，知道最高位。  