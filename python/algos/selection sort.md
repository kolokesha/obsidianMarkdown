#algos
![Exponentially Easy Selection Sort | by Vaidehi Joshi | basecs | Medium|600](https://miro.medium.com/max/2944/1*S-wdMkkaX3Gr4bQrbqu_1Q.jpeg)

НО данная реализация создаёт отдельный arr куда добавляет значения а не переставляет его в массиве

```python
def findBiggest (arr):
    return(max(arr))

def selectionSort(arr):
    newArr = []
    for i in range(len(arr)):
        biggest = findBiggest(arr)
        newArr.append(biggest)
        arr.remove(biggest)
    return newArr

def main():
    user_arr = input().split()
    arr = list(map(int, user_arr))
    print(selectionSort(arr))

if __name__ == '__main__':
    main()