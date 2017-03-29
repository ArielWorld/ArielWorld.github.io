---
layout: post
title:  "js实现一些简单算法"
date:   2017-03-12
excerpt: ""
tag:
- 算法
comments: true
---

算法真是致命。今天看不完了，写点别的吧，关于数组存储和链表存储的区别：
数组存储就是内存地址一个挨着一个的存储，查询速度快，但是增加或删除慢，因为会造成被增加或删除的项后边的所有数据内存地址的改变；
链式存储是以节点的形式存储，这个节点指向下个节点，但他们的内存地址不是挨着的，所以查询慢，但是修改快，只需要改变节点就可以了。

//终于开始梦寐以求的算法学习了~~，才发现男神又写过了的，果然是男神

排序有稳定和不稳定两种，不稳定的排序算法可能会在相等的键值中改变记录的相对次序，稳定性排序不会。一般而言，好的性能是O(nlog n)，且坏的性能是O(n^2)。对于一个排序理想的性能是O(n)。
常见的几种排序方法：
1.冒泡排序，稳定，O(n^2);
2.插入排序，稳定，O(n^2);
3.堆排序，不稳定，O(nlog n);
4.归并排序，稳定，O(nlog^2n);
5.快速排序，不稳定，平均O(nlog n)，最坏O(n^2)。

以下排序均默认升序。

js快速排序，思想：
1.在数据集之中，选择一个元素作为"基准"（pivot）。
2.所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
3.对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

```js
var quickSort = function(array){
    //array.length会变~~不要存起来
    if(array.length<=1){return array;}
    var pivotIndex = Math.floor(array.length/2);
    var pivot = array.splice(pivotIndex,1)[0];
    var left = [];
    var right = [];
    for(var i =0;i<array.length;i++){
        if(array[i]<pivot){
            left.push(array[i]);
        }else{
            right.push(array[i]);
        }
    }
    return quickSort(left).concat([pivot],quickSort(right));
};
```


//失恋了，心情糟糕的很，下面的就随便写写

冒泡排序:
比较相邻的元素。如果第一个比第二个大，就交换他们两个。
对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
针对所有的元素重复以上的步骤，除了最后一个。
持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

```js
function bubbleSort(arr){
    var temp;
    for(var i=0;i<arr.length;i++){
        for(var j=0;j<arr.length-1-i;j++){
            if(arr[j]>arr[j+1]){
                temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
    return arr;
}
```

插入排序：
设数组为a[0…n]。
1.将原序列分成有序区和无序区。a[0…i-1]为有序区，a[i…n] 为无序区。（i从1开始）
2.从无序区中取出第一个元素，即a[i]，在有序区序列中从后向前扫描。
3.如果有序元素大于a[i]，将有序元素后移到下一位置。
4.重复步骤3，直到找到小于或者等于a[i]的有序元素，将a[i]插入到该有序元素的下一位置中。
5.重复步骤2~4，直到无序区元素为0。

```js
function insertSort(arr){
    for(var i=1;i<arr.length;i++){
        var key = arr[i];
        var j = i-1;
        while(j>=0&&arr[j]>key){
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
    return arr;
}
```

二分插入排序：

设数组为a[0…n]。
1.将原序列分成有序区和无序区。a[0…i-1]为有序区，a[i…n] 为无序区。（i从1开始）
2.从无序区中取出第一个元素，即a[i]，使用二分查找算法在有序区中查找要插入的位置索引j。
3.将a[j]到a[i-1]的元素后移，并将a[i]赋值给a[j]。
4.重复步骤2~3，直到无序区元素为0。

```js
function binaryInsertSort(array) {
  for (var i = 1; i < array.length; i++) {
    var key = array[i], left = 0, right = i - 1;
    //只要满足条件就会循环
    while (left <= right){
         var middle = parseInt((left + right) / 2);
          if (key < array[middle]){
                right = middle - 1;
            }else{
                left = middle + 1;
            }
    }
     for (var j = i - 1; j >= left; j--){
        array[j + 1] = array[j];
     }
     array[left] = key;
  }
  return array;
}
```

//现在有些难过，剩下的以后再研究吧

二分法还可以用来数组查值，不过今儿累了，明日再写。

```js
//二分查找法
Array.prototype.binarySearch = function(key){
    var index = -1,
        left = 0,
        right = this.length-1;
    while(left<=right){
       var  mid = parseInt((left+right) / 2);
        if(this[mid] == key){
            index = mid;
            break;//切记要跳出循环，否则，会像我一样浏览器崩溃了~~
        }else if(this[mid]>key){
            right = mid-1;
        }else{
            left = mid+1;
        }
    }
    return index;
}
```

参考：[排序图解：js排序算法实现](http://www.cnblogs.com/wteam-xq/p/4752610.html)
[常用排序算法之JavaScript实现](http://www.cnblogs.com/ywang1724/p/3946339.html)