#basics

# Basics Data types
1. [[Type int]] example 12342
2. [[Float]] 3.3424
3. Complex - example '3.4231e-10'
4. [[String]] 'example string'
5. [[List]] - ['1', '123', 123, 'string', ['1', 1]]
6. [[Set]] - {'1', 2,}
7. [[Dict]] - {'key':'value'}
8. [[Tuple]] - (2,6)
9. [[None type]] - None type



## mutables

 Изменяемые (mutable) | Неизменяемые (unmutable)
------------ | ------------
list | int
set | float
dict | complex
dict['key'] - ключ | str
 -| tuple

### Хешируемые объекты

Для быстрого поиска и сравнения данных, сохранённых в памяти, Python применяет **хеширование** данных — кодирование, которое преобразует любое значение в целое число.

Значения неизменяемых типов данных хранятся в памяти в хешированном виде. С момента создания блока памяти и до его утилизации хеш-значение не может быть изменено: таковы законы этого мира.

Получить хеш-значение для переменной можно встроенной функцией `hash()`

Для изменяемых типов данных функция `hash()` не работает: данные изменяемых типов не хешируются.



``` python
list_hash = hash([1,2,3,4,5])
print(f"Хеш для списка [1,2,3,4,5] будет равен {list_hash}")
# Вывод в терминал: TypeError: unhashable type: 'list'
# Все сломалось: списки не хешируются!
```

**Неизменяемые объекты**

-   Подходят для безопасного хранения данных (в ходе программы значения не будут изменены, если случайно не переопределить переменную).
-   Скорость обращения к неизменяемым объектам для некоторых операций гораздо выше, чем при работе с изменяемыми объектами.

**Изменяемые объекты**

-   При работе с динамически изменяющимися данными объекты изменяемого типа упрощают работу с памятью: не приходится постоянно создавать всё новые и новые объекты в памяти; это экономит время и ресурсы.