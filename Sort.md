# Quick Sort
*      1. 选择数组的最左一个作为pivot
       2. 小于pivot的放左边（不一定有序），大于pivot的放右边，等于pivot放中间
       3. 这一过程称为分区的过程。递归执行上述过程直到有序。
* 时间复杂度：O(nlogn)
* 空间复杂度：快速排序使用递归，递归使用栈，因此它的空间复杂度为O(logn)
* 稳定性：快速排序无法保证相等的元素的相对位置不变，因此它是不稳定的排序算法
* [其他排序](https://mp.weixin.qq.com/s/vn3KiV-ez79FmbZ36SX9lg)
* [参考图解](https://www.jianshu.com/p/a68f72278f8f)
```java
import java.util.Arrays;

public class QuickSort {

    public int[] sort(int[] sourceArray) {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        return quickSort(arr, 0, arr.length - 1);
    }

    private int[] quickSort(int[] arr, int left, int right) {
        if (left < right) {
            int partitionIndex = partition(arr, left, right);
            quickSort(arr, left, partitionIndex - 1);
            quickSort(arr, partitionIndex + 1, right);
        }
        return arr;
    }

    private int partition(int[] A, int left, int right) {
        // 设定基准值（pivot）
        int pivot = A[left];
        // 从左边开始数起
        while(left<right){
            // 小于等于pivot的数都移动到左边，即是从右找到第一个小于等于pivot的数与left互换
            while(left<right && A[right] > pivot) right--;
            A[left] = A[right];
            // 大于pivot的数都移动到右边，即是从左找到第一个大于pivot的数与right互换
            while(left<right && A[left] <= pivot) left++;
            A[right] = A[left];
            }
        // 此时left==right，都指向第一个大于pivot的数，将这个数（即loop里最后一个A[left]）与pivot互换
       A[left] = pivot;
       return left;
    }
    
    public static void main(String[] args) {
        QuickSort qs = new QuickSort();
        int[] arr = { 9, 8, 7, 6, 5, 4, 3, 2, 1 };
        int[] arrEqual = { 2, 1, 3, 2, 2, 2, 3, 2 };
        int[] arrComplex = { 4, 3, 2, 6, 5, 6, 2, 7, 5, 8, 4 };
        System.out.println(Arrays.toString(qs.sort(arr)));
        System.out.println(Arrays.toString(qs.sort(arrEqual)));
        System.out.println(Arrays.toString(qs.sort(arrComplex)));
    }
}
```
