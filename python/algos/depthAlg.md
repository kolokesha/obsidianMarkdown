#algos 

**Поиск в ширину с графами**

![[Pasted image 20210806233753.png]]

      
Поиск в ширину выполняется за время О(количество

людей+ количество ребер), что обычно записывается в форме O(V+E) (Vкол

ичество вершин , Е - количество ребер).

```python
from collections import deque  
from igraph import Graph  
  
def search(name):  
    search_queue = deque()  
    print(search_queue)  
    search_queue += name  
 searched = []  
    while search_queue:  
        person = search_queue.popleft()  
        if not person in searched:  
            if person_is_seller(person):  
                print(person + 'is a mango dealer')  
                return True  
 else:  
                search_queue += person  
                searched.append(person)  
        return False  
  
  
def person_is_seller(name):  
    return name[-1] == 'm'  
  
  
if __name__ == '__main__':  
    search('you')
```

Понятно как работает, но нид то реализовать ;D
#для_реализации