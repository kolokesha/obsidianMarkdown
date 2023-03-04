#basicsgolang #slice

## Слайсы

Раз длина массива — это часть его типа, то массивы не подходят для хранения коллекций данных динамического размера. Эту задачу решает другой тип данных, который гораздо чаще используется на практике, — **слайс (от англ. slice)**, или срез. Кроме того, слайс избавит нас от проблемы копирования массивов для присваивания.

**Слайс** — это последовательность переменной длины, состоящая из элементов одного типа. Тип слайса записывается как тип массива без указания размера. Можно инициализировать переменную типа «слайс» значениями, но, в отличие от массива, переменная без инициализации равна `nil`.


```go
var mySlice []int 
```

Слайс очень похож на `list` в языке Python, но имеет свои особенности.

Слайс — это обёртка над указателем массива, и в Go слайс используется как структура следующего вида:

-   указатель на первый элемент базового массива — `ptr`;
-   длина слайса — `len`, количество элементов в слайсе;
-   ёмкость слайса — `cap`, количество элементов в массиве.

![image](https://pictures.s3.yandex.net/resources/basics_go_3.2_1654600417.png)

Параметры слайса `len` и `cap` могут быть получены с помощью вызова соответствующих встроенных функций `len()` и `cap()`.

Если просто объявить такую структуру, то она будет равна `nil`.

Для создания слайса используется встроенная функция `make()`:


```go
    mySlice := make([]TypeOfelement, LenOfslice, CapOfSlice)
    mySlice := make([]int, 0) // слайс [], базовый массив []
    mySlice := make([]int, 5) // слайс [0 0 0 0 0], базовый массив [0 0 0 0 0]
    mySlice := make([]int, 5, 10) // слайс [0 0 0 0 0], базовый массив [0 0 0 0 0 0 0 0 0 0] 
```

Аргументы функции `make`:

1.  Тип слайса (пустые квадратные скобки и тип элемента слайса).
2.  Длина слайса. Если не передана, то по умолчанию равна нулю.
3.  Ёмкость слайса — размер базового массива. Если значение не передано, то по умолчанию равна длине слайса.

Функция `make` создаёт массив длиной `cap` и записывает указатель на него в структуру слайса. Также она заполняет поля `len` и `cap` в этой структуре и возвращает её в виде переменной типа «слайс».

Даже если `len` и `cap` переданы как нулевые, сама структура уже не будет равна `nil`. Она будет выделена в памяти, и указатель на базовый массив получит значение, отличное от `nil`.

Если передать в функцию `make` параметр `cap` меньше `len`, то будет вызвана ошибка компиляции или паника во время исполнения.

Слайс может быть создан из композитного литерала так же, как и массив. Единственное отличие — не указываем размер массива:


```go
s := []int{1, 2, 3}  // [1 2 3] 
```

Длина и ёмкость слайса будут равны композитному литералу.

Новый слайс может быть создан на основе уже существующего слайса или массива. Для этого используется операция взятия слайса. Она выполняется с помощью двух скобок с двоеточием `[i:j]`, где `i` — индекс первого элемента нового слайса, а `j` — индекс следующего элемента, **не входящего** в новый слайс.

Допускается не указывать `i` и `j` — в таком случае `i` по умолчанию будет равен 0, `j` — равен длине массива или слайса.

Таким образом, `[:]` вернёт слайс всего массива, `[:k]` — от начала и до k-го элемента, `[k:]` — от k-го элемента до конца массива.

`i` и `j` должны быть неотрицательны и не больше `len`, причем `i` меньше или равно `j`. Если эти условия не будут выполнены, возникнет ошибка компиляции или паника.

Рассмотрим пример с массивом среднесуточных температур из раздела про массивы.


```go
    weekTempArr := [7]int{1, 2, 3, 4, 5, 6, 7}
    workDaysSlice := weekTempArr[:5]
    weekendSlice := weekTempArr[5:]
    fromTuesdayToThursDaySlice := weekTempArr[1:4] 
    weekTempSlice := weekTempArr[:]
    
    fmt.Println(workDaysSlice, len(workDaysSlice), cap(workDaysSlice)) // [1 2 3 4 5] 5 7
    fmt.Println(weekendSlice, len(weekendSlice), cap(weekendSlice)) // [6 7] 2 2 
    fmt.Println(fromTuesdayToThursDaySlice, len(fromTuesdayToThursDaySlice), cap(fromTuesdayToThursDaySlice)) // [2 3 4] 3 6 
    fmt.Println(weekTempSlice, len(weekTempSlice), cap(weekTempSlice)) // [1 2 3 4 5 6 7] 7 7 
```

### Изменение размеров слайса

Как вы уже знаете, слайс — последовательность элементов динамического размера.

Уменьшение размера слайса производится через операцию взятия слайса. Результат взятия можно присвоить этому же слайсу:


```go
    s := []int{1,2,3} // [1 2 3]
    s = s[:len(s)-1] // [1 2] 
```

Ёмкость массива при этом не изменяется.

Для добавления элементов к слайсу используется встроенная функция `append`. Она принимает переменную типа «слайс» и одну или несколько переменных типа элемента слайса, затем возвращает новый слайс, который состоит из копии переданного слайса и переданных в него элементов.

Внимание: `append` не изменяет переданный ей слайс, а создаёт новый на основе переданного.

Рассмотрим пример:


```go
    a := []int{1, 2, 3, 4}
    b := a[2:3]   // b = [3]
    b = append(b, 7)
    fmt.Println(a, len(a), cap(a)) // [1 2 3 7] 4 4
    fmt.Println(b, len(b), cap(b)) // [3 7] 2 2
    b = append(b, 8, 9, 10)
    b[0] = 11
    fmt.Println(a, len(a), cap(a)) // [1 2 3 7] 4 4
    fmt.Println(b, len(b), cap(b)) // [11 7 8 9 10] 5 6 
```

Слайс `a` изменился после выполнения первого `append`, так как слайс `b` хоть и увеличил свою длину, но не вышел за пределы базового массива.

Дело в механике `append`: если ёмкость слайса позволяет разместить добавляемые элементы (то есть разница между длиной слайса и его ёмкостью больше или равна количеству размещаемых элементов), то `append` возвращает новый слайс, который получен из существующего слайса добавлением новых элементов внутри базового массива.

Если же ёмкость слайса не позволяет разместить эти элементы, то создаётся новый базовый массив подходящего размера, в него копируются все элементы переданного слайса и добавляются новые. Именно поэтому в примере после второго `append` слайс `b` ссылается на новый базовый массив. Разница между ёмкостью нового базового массива и его длиной зависит от количества элементов. В примере длина базового массива для `b` равна 5, а ёмкость — 6.

Для 1000 элементов разница будет другой:


```go
    a := make([]int, 1000)
    b := append(a, 7)
    fmt.Println(len(a), cap(a))  // 1000 1000
    fmt.Println(len(b), cap(b))  // 1001 1536 
```

Такой алгоритм работы применяется не только в Go, но и в Python, С++ и многих других языках программирования. Это связано с тем, что выделение нового участка памяти во время работы программы — достаточно дорогостоящая операция с точки зрения затрат вычислительных ресурсов, и выгоднее держать немного памяти про запас. Ёмкость базового массива как раз и задаёт такой запас.

Чтобы соединить два слайса, нужно распаковать слайс `append(a,b...)`. Функция принимает некоторое количество отдельных элементов и преобразует слайс в список через распаковку.

Рассмотрим ещё несколько примеров.


```go
s := make([]int, 4, 7) // [0 0 0 0], len = 4 cap = 7
// 1. Создаём слайс s с базовым массивом на 7 элементов. 
// Четыре первых элемента будут доступны в слайсе.

slice1 := append(s[:2], 2, 3, 4)  
fmt.Println(s, slice1) // [0 0 2 3] [0 0 2 3 4]
// 2. Берём слайс из первых двух элементов s и добавляем к ним три элемента.
// Так как суммарная длина полученного слайса (len == 5) меньше ёмкости s[:2] (cap == 7), 
// то базовый массив остаётся прежним.
// Слайс s тоже изменился, но его длина осталась прежней

slice2 := append(s[1:2], 7) 
fmt.Println(s, slice1, slice2) // [0 0 7 3] [0 0 7 3 4] [0 7]
// 3. Здесь также базовый массив остаётся прежним, изменились все три слайса

slice3 := append(s, slice1[1:]...)
fmt.Println(len(slice3), cap(slice3))  // 8 14
// 4. Длина s и slice1[1:] равна 4, длина нового слайса будет равна 8,  
// что больше ёмкости базового массива.
// Будет создан новый базовый массив ёмкостью 14,
// ёмкость нового базового массива подбирается автоматически 
// и зависит от текущего размера и количества добавленных элементов

// 5. Легко проверить, что slice3 ссылается на новый базовый массив
s[1] = 99
fmt.Println(s, slice1, slice2, slice3) 
// [0 99 7 3] [0 99 7 3 4] [99 7] [0 0 7 3 0 7 3 4] 
```

Так этот процесс выглядит на картинке:

![image](https://pictures.s3.yandex.net/resources/3.2.slices_1638795136.png)

Здесь не очень понятно, будут ли новые слайсы ссылаться на тот же базовый массив или отправят свои копии в новый массив. Поэтому на практике функцию `append` рекомендуют лишь для присвоения слайса самому себе: `s = append(s, b)`.

Чтобы использовать слайсы, нужно понимать механизм их работы. В противном случае вы будете получать ошибки, которые очень трудно найти. Старайтесь избегать ситуаций, когда на базовый массив ссылается несколько слайсов и происходит добавление или изменение элементов.

Операция взятия слайса поддерживает и третий параметр: `[low:high:max]` — третьим параметром указывается ёмкость базового массива, необходимая для создания нового слайса. При этом `max` должна быть меньше или равна ёмкости базового массива или слайса. Вряд ли вы встретите аналогичный пример на практике, но знать про него будет полезно.

## Присваивание слайса и передача в функции

Присвоение друг другу переменных слайса даже самого внушительного размера не потребляет больших вычислительных мощностей, потому что сама по себе структура слайса всегда содержит всего три поля: `ptr`, `len` и `cap`. Однако надо держать в уме, что эти переменные ссылаются на один и тот же массив, поэтому изменение в данных одного слайса может повлечь за собой изменение другого.

При передаче слайса в аргументы функции структура слайса копируется в локальную переменную внутри функции. Это позволяет изменить данные внутри слайса, переданного в функцию. Но если нужно добавить или удалить элементы из слайса, то эти изменения затронут только локальную переменную слайса.

Для примера возьмём пару функций из стандартной библиотеки:

```go
    s := []int{5, 4, 1, 3, 2}
    sort.Ints(s)
    fmt.Println(s) // [1 2 3 4 5] 
```

Функция `sort.Ints` сортирует полученный слайс целых чисел по возрастанию. Она не меняет размер и ёмкость слайса, поэтому может спокойно работать с ним.



```go
    // Пример взят из стандартной библиотеки
    import (
    "bytes"
    "fmt"
)

func main() {
    bSlice := []byte(" \t\n a lone gopher \n\t\r\n")
    fmt.Printf("%s", bytes.TrimSpace(bSlice)) // a lone gopher
    fmt.Printf("%s", bSlice)  // \t\n a lone gopher \n\t\r\n 
    
} 
```

Функция `bytes.TrimSpace` принимает слайс байт и возвращает новый слайс байт, откуда были удалены начальные и конечные пробельные символы. Размер слайса должен измениться, а значит, `bSlice` останется нетронутым. В итоге `bytes.TrimSpace` подарит нам новый слайс.

## Копирование слайсов

Для копирования элементов из одного слайса в другой применяется функция `copy([]T dest, []T src)`, где `dest` — это слайс-приёмник, а `src` — слайс-источник. Эта функция только перезаписывает элементы, поэтому количество скопированных элементов будет равно меньшей длине из двух слайсов.


```go
var dest []int
dest2, dest3 := make([]int, 3),  make([]int, 5)
src := []int{1, 2, 3, 4}
copy(dest, src)
copy(dest2, src)
copy(dest3, src)
fmt.Println(dest, dest2, dest3, src ) // [] [1 2 3] [1 2 3 4 0] [1 2 3 4] 
```

### Обход слайсов и доступ к элементам

Обход слайсов и доступ к элементам слайса происходят точно так же, как и для массивов. Чтобы добраться до элементов по индексу, используются квадратные скобки `[]`, циклы `for` и `for range`.

### Полезные приёмы для работы со слайсами

В отличие от других языков программирования, Go не щеголяет обилием функций для работы со слайсами. Гоферы выкрутились и придумали несколько приёмов, которые позволяют решать частые задачи.

Удаление последнего элемента слайса:


```go
    s := []int{1, 2, 3}
    if len(s) != 0 { // защищаемся от паники
        s = s[:len(s)-1]
    }
    fmt.Println(s) // [1 2] 
```

Удаление первого элемента слайса:


```go
    s := []int{1,2,3}
    if len(s) != 0 { // защищаемся от паники
        s = s[1:]
    } 
    fmt.Println(s) // [2 3] 
```

Удаление элемента слайса с индексом `i`:


```go
    s := []int{1,2,3,4,5}
    i := 2
    
    if len(s) != 0 && i < len(s) { // защищаемся от паники
        s = append(s[:i], s[i+1:]...)
    } 
    fmt.Println(s) // [1 2 4 5] 
```

Сравнение двух слайсов:


```go
    
  s1 := []int{1,2,3}
    s2 := []int{1,2,4}
    s3 := []string{"1","2","3"}
    s4 := []int{1,2,3}

    fmt.Println(reflect.DeepEqual(s1,s2)) // false
    fmt.Println(reflect.DeepEqual(s1,s3)) // false
    fmt.Println(reflect.DeepEqual(s1,s4)) // true 
```

## Заключение

Слайсы широко применяются в Go для реализации коллекций однотипных элементов, но требуют грамотного применения: без понимания их устройства вы можете нарваться на каверзные ошибки.

## Дополнительные материалы

-   [Go Slice Tricks Cheat Sheet](https://ueokande.github.io/go-slice-tricks/)
-   [Digital Ocean. Массивы и срезы в Go](https://www.digitalocean.com/community/tutorials/understanding-arrays-and-slices-in-go-ru)