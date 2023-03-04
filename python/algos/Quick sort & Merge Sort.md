#algos 

Quick Sort

![[Pasted image 20210607182532.png]]

MergeSort

```python
import time
start_time = time.time()
def quickSort(arr):
    if len(arr) < 2:
        return arr
    else:
        curr = arr[len(arr) // 2]
        less = [i for i in arr[1:] if i <= curr]
        over = [i for i in arr[1:] if i > curr]
    return quickSort(less) + [curr] + quickSort(over)
print(quickSort([1, 1, 1, 1, 1, 12, 22, 11, 334, 31, 23, 43]))
print("--- %s seconds ---" % (time.time() - start_time))
```
QuickSort

```python
import time
start_time = time.time()
def quickSort(arr):
    if len(arr) < 2:
        return arr
    else:
        curr = arr[0]
        less = [i for i in arr[1:] if i <= curr]
        over = [i for i in arr[1:] if i > curr]
    return quickSort(less) + [curr] + quickSort(over)
print(quickSort([1, 1, 1, 1, 1, 12, 22, 11, 334, 31, 23, 43]))
print("--- %s seconds ---" % (time.time() - start_time))
```

Разница в том что в quickSort берём первый элемент или последний, в Merge Sort берём середину

1). Сложность времени: все - O (nlogn), но быстрая сортировка - в среднем O (nlogn), сортировка слиянием - лучшая, а худшая - O (nlogn)

Расчет средней временной сложности быстрой сортировки: если есть 3 числа, есть 6 перестановок, ожидаемая временная сложность составляет: 1/6 \* (сумма времени каждой перестановки)

2). Сложность пространства: быстрая сортировка требует O (1) дополнительного пространства, сортировка слиянием должна занимать O (n) дополнительного пространства

3) Стабильность сортировки: быстрая сортировка - это нестабильный алгоритм сортировки. Сортировка слиянием - это стабильный алгоритм сортировки

4) Идея «разделяй и властвуй»: быстрая сортировка сначала упорядочивается глобально, а затем - локально. Сортировка слиянием сначала частично упорядочивается, а затем упорядочивается глобально.
