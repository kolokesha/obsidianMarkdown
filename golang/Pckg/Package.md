#basicsgolang #package #modules

В этой теме вы:

-   узнаете, что такое пакеты, как их определять и импортировать;
-   познакомитесь с принципами организации кода в Go-проектах;
-   научитесь использовать модули и изучите их возможности;
-   узнаете, как работать с внешними зависимостями;
-   познакомитесь с основными утилитами для работы с кодом и поддержки Go-проектов.

# Пакеты и импорт

Чем сложнее программа и чем больше людей над ней работают, тем важнее правильная организация исходного кода и понятная для всех архитектура проекта.

Любую крупную задачу можно разбить на подзадачи и реализовать их в виде пакетов. Грамотное разделение проекта на пакеты и размещение их в соответствующих директориях позволяет:

-   подключать к работе больше людей, так как разработчики могут реализовывать пакеты независимо друг от друга;
-   тестировать и отлаживать компоненты (пакеты) сразу после их образования, создавая тесты внутри пакета;
-   поддерживать и вносить изменения в дальнейшем, так как нужный код легко найти.

## Пакет

**Пакет** — это единица компиляции, пространства имён и импорта. Весь код на языке Go находится в каком-либо пакете.

Проще говоря, пакет — это набор файлов с исходным кодом, который находится в одной папке проекта. Пакеты позволяют логически разделить ваш проект на компоненты. Все элементы кода (типы, константы, переменные, функции) доступны внутри пакета, как если бы они были объявлены в одном файле. В каждом файле исходный код на языке Go должен начинаться с объявления пакета: ключевого слова `package` и имени пакета.

Скопировать кодGO

```
package main

import "fmt"

func main() {
    fmt.Println(`Hello, world!`)
} 
```

В директории у всех файлов должно быть одинаковое имя пакета, потому что компилятор будет обрабатывать сразу все файлы директории (если не указать на конкретный) и при разных именах пакетов возникнет ошибка. Однако из этого правила есть исключение — файлы, название которых заканчивается на `_test.go`, могут иметь другое имя пакета, так как они игнорируются при сборке программы.

Подход «одна директория — один пакет» упрощает работу и дальнейшую поддержку программ. Строго рекомендуется, чтобы название пакета совпадало с именем папки, так как с ними будет проще работать.

Если нужно создать исполняемый файл, у пакета должно быть имя `main` и одноимённая функция в одном из файлов. В остальных случаях, если пакет будет использоваться как библиотека, желательно, чтобы имя пакета отражало его назначение.

### Как лучше называть пакет и его элементы

Вместо создания одного большого универсального пакета старайтесь разбивать код на мелкие пакеты и присваивать каждому из них понятное имя.

1.  Записывайте имя пакета строчными буквами. Хорошо, если оно будет коротким, но информативным: другим программистам должно быть понятно, что делает этот пакет.
2.  Подбирайте пакету уникальное имя в рамках репозитория. В противном случае при импорте нескольких пакетов с одинаковыми именами придётся использовать алиасы.
3.  Не пишите в имени пакета общие слова: `util`, `base`, `tools`, `lib`, `common`. Обычно эти имена ничего не рассказывают о реальном назначении пакета и только засоряют пространства имён. Помните, что имя пакета будет использоваться в коде.
4.  Не используйте множественное число. Пакеты `strings`, `bytes`, `errors` в стандартной библиотеке Go названы так, чтобы избежать конфликта с типами.

Когда будете называть элементы пакета, помните главное правило: при импорте пакета доступны только те определения, имена которых начинаются с заглавной буквы. В примере ниже при импорте пакета можно вызвать `mypkg.Process()`, но не `mypkg.calculate()`. Это правило относится к функциям, методам, константам, типам и глобальным переменным.

Скопировать кодGO

```
package mypkg

func Process() {
    ...
}

func calculate() {
    ...
} 
```

Не используйте слова из названия пакета при именовании экспортируемых объектов. Например, если есть пакет `md` для работы с форматом Markdown, функцию конвертации в HTML можно так и назвать — `HTML`. Не стоит называть функцию `MarkdownToHTML`: имя пакета уже указывает на то, с чем вы работаете.

#### Пути пакета

Кроме имени пакета, важно правильно выбрать директорию, где он будет храниться. Компилятор пойдёт по этому пути, чтобы произвести импорт.

Раньше все исходные файлы должны были находиться в поддиректории `GOPATH\src`. Здесь `GOPATH` — это переменная окружения, которая определяет рабочее пространство, где хранятся исходники и двоичные файлы. По умолчанию она равна директории установки Go: если при импорте указан пакет `company\pkg`, он должен находиться в `GOPATH\src\company\pkg`.

Сейчас такой подход признан устаревшим, поэтому рекомендуется использовать модули (modules). Подробнее о работе с модулями расскажем в следующих уроках, а здесь будет краткая версия: **модуль** — это законченная библиотека или приложение, которое может содержать внутренние пакеты и импортировать внешние. Модуль вы можете разместить в любой директории, независимо от пути установки Go. Если ваш пакет представляет собой отдельную библиотеку или приложение, оформляйте его в виде модуля и размещайте, где вам удобно.

Cтрогих правил по организации директорий нет, но лучше начать путь с домена или имени автора, чтобы избежать конфликтов имён при публичном использовании пакетов. Например, вы можете создать директорию `golang` и размещать там все Go-проекты, для личных закрытых проектов создать отдельную поддиректорию, а публичные размещать сразу в поддиректории соответствующего репозитория. В этом случае пакеты с GitHub будут расположены в поддиректориях вида `github.com/author/packagename`.

![image](https://pictures.s3.yandex.net/resources/5.1.packages_1640005281.png)

## Экспорт

Как было сказано выше, все элементы кода доступны внутри пакета. Но как сделать их доступными извне? Для этого в Go существует концепция экспорта.

Любой элемент (тип, константа, переменная, функция) является экспортируемым, то есть доступным внешним пакетам для импорта, если его имя начинается с большой буквы. В противном случае такой элемент доступен только внутри пакета. Это сродни модификаторам `public` и `private` из С++/Java или `static` из С++.

Пример экспортируемых элементов:

Скопировать кодGO

```
var ParsedString string

func Print (s string) {}

const Red = 3

type MyStruct struct {
    a int
} 
```

И неэкспортируемые:

Скопировать кодGO

```
func someTestFunc()

const green = 5
 
```

Экспортируемые элементы представляют собой внешний интерфейс вашего пакета. Пусть он будет минималистичным. Если элемент не нужен вне пакета, лучше сделать его неэкспортируемым.

Также с особой осторожностью следует относиться к экспорту переменных: возможно, будет лучше сделать функции, меняющие ваши переменные, чтобы код не становился негибким и опасным. Вам будет труднее делать изменения в коде пакета, если внешний код использует переменные из вашего пакета.

## Импорт

Чтобы один пакет мог использовать другой, его надо импортировать. Импорт пакета чем-то похож на аналогичный процесс в Python. Он выполняется с помощью ключевого слова `import`.

Указывать `import` можно для каждого пакета, но чаще перечисляют все пакеты внутри круглых скобок. `import` должен идти сразу после объявления `package`.

Скопировать кодGO

```
package main

import "fmt"
import (
    "encoding/json"
    "strings"

    "github.com/yuin/goldmark"
    "golang.org/x/crypto/bcrypt"
    "gopkg.in/yaml.v2"
) 
```

Пакеты можно разбивать пустыми строками на группы: например, стандартная библиотека, внутренние пакеты и внешние пакеты. При этом пакеты внутри группы лучше отсортировать по алфавиту. Подключите автоматический вызов стандартной утилиты форматирования `gofmt` к своей IDE, чтобы исправлять подобные недочёты. Если у пакета несколько версий, можно указывать нужную версию после имени пакета.

Как было сказано выше, импортируются только экспортируемые переменные. Если пакеты при импорте образуют цикл, то есть импортируют друг друга напрямую или через другие пакеты, компилятор выдаст ошибку. Например, если у нас есть три пакета, то такая последовательность импорта недопустима — компилятор просто впадёт в цикл, обходя пакеты:

Скопировать кодGO

```
package A

import "B" 
```

Скопировать кодGO

```
package B

import "C" 
```

Скопировать кодGO

```
package C

import "A" 
```

При обращении к импортируемым объектам нужно указывать через точку имя пакета и имя элемента: `fmt.Println(...), yaml.Marshal(...)`.

### Переименование импорта

Может возникнуть ситуация, когда понадобится импортировать два пакета с одинаковыми именами: например, `crypto/rand` и `math/rand`. Тогда нужно для одного из пакетов указать другое имя (алиас). Кроме этого случая, альтернативное имя при импорте бывает полезным, если у пакета длинное имя. Что касается нового имени пакета, то оно указывается через пробел перед полным именем.

Скопировать кодGO

```
import (
    hl "github.com/yuin/goldmark-highlighting"
) 
```

Согласитесь, вызов `hl.NewHighlighting(...)` не загромождает код, в отличие от `goldmark-highlighting.NewHighlighting(...)`. Однако не стоит увлекаться переименованиями без явной нужды.

### Порядок импорта

Рассмотрим подробнее, как выполняется импорт пакетов.

1.  При компиляции программы компилятор начинает с пакета `main`. Если в `main` есть импорты каких-либо пакетов, то он переходит к ним и компилирует их, до тех пор пока не скомпилируются все необходимые пакеты для сборки программы.
2.  Затем компилятор компилирует пакет `main` и собирает основное приложение, а далее в процессе выполнения программы произойдёт следующее: 1. В том порядке, в котором пакеты были проимпортированы, будут инициализироваться переменные пакета. 2. После будут выполнены функции `init()` внутри каждого пакета. Функций `init()` может быть несколько, и они выполнятся в том порядке, в котором были объявлены. 3. И после их выполнения наступит очередь функции `main`.

Таким образом, часть кода по инициализации может быть выполнена ещё до запуска основной программы. Это очень удобная особенность, и в процессе работы вы, скорее всего, с ней столкнётесь.

### Пустой импорт

Если импортировать пакет, но не использовать его внутри файла, компилятор выдаст ошибку. Однако бывают ситуации, когда нужно, чтобы импортируемый пакет вызвал функцию `init` для инициализации данных. В этом случае указывается знак подчёркивания `_` вместо альтернативного имени импортируемого пакета.

В качестве примера можно привести очень удобный пакет `embed` из стандартной библиотеки. Он позволяет инициировать значения строковых переменных содержимыми файла.

Скопировать кодGO

```
import _ "embed"

//go:embed insert.sql
var queryInsert string 
```

В первом случае можем получить пустой импорт, когда пакет не используется напрямую, не вызываются функции из него, а импортируются только аннотации в комментариях.

Второй случай — применение в процессе разработки. Go запрещает компилировать код, в котором есть неиспользованные импорты, но иногда при отладке импорт нужно оставлять. Тогда он может быть помечен как пустой. Естественно, этот код используется только в отладочных целях и стоит держать его подальше от вашей кодовой базы.

## Организация кода

Go не накладывает ограничений на структуру проекта и расположение пакетов. Можно организовывать файлы в проекте как угодно, но есть рекомендации, придерживаясь которых вы сделаете свой проект понятным для других программистов.

Существует подход, который называется **Standard Go Project Layout**. В нём код проекта организуется в виде следующих директорий:

### cmd

Если в проекте будет несколько бинарных файлов, создайте для них поддиректории в `cmd`. Имена поддиректорий должны соответствовать именам исполняемых файлов.

Скопировать кодTXT

```
- cmd
    - client
        main.go
    - server
        main.go 
```

Если в проекте один исполняемый файл, исходные файлы можно разместить прямо в директории проекта. Старайтесь использовать один-два исходных файла. Остальной код нужно вынести в отдельные пакеты.

### internal

Директория `internal` содержит внутренние пакеты Go-проекта. На уровне компилятора запрещён импорт таких пакетов извне родительской директории `internal`. Например, пакет `.../root/client/internal/a/b` можно импортировать только в файле дерева директорий, которые начинаются с `.../root/client`, и нельзя в `.../root/server` или другом репозитории.

Рекомендуется размещать в директории `internal` весь основной исходный код программы, разбитый на поддиректории с соответствующими пакетами. В зависимости от сложности проекта пакеты могут иметь разный уровень вложенности.

### pkg

Директорию `pkg` определите для пакетов, которые можно использовать в других проектах. Предпочтительнее для публичных проектов заводить отдельные репозитории.

### vendor

Директория `vendor` содержит внешние пакеты. C появлением в Go модулей все зависимости хранятся в кеше модуля. Поэтому директорию `vendor` можно использовать на старых версиях Go или в том случае, если вы хотите быть уверены, что все зависимости находятся внутри директории проекта.

### test

Как правило, в каждом пакете есть один или несколько тестовых файлов `name_test.go`. Директорию `test` можно использовать для комплексного тестирования с привлечением дополнительных инструментов.

### docs

Директория `docs` предназначена для ведения документации по проекту. Это может быть документация для пользователей или дополнение к документации, которую автоматически генерирует `godoc`.

### Прочие директории

Вот ещё варианты директорий, которые встречаются в Go-проектах:

-   `api` — дополнительные файлы для сервисов с API.
-   `assets` — дополнительные файлы-ресурсы. Например, картинки.
-   `build` — файлы для упаковки и непрерывной интеграции.
-   `configs` — файлы конфигураций.
-   `deployments/deploy` — файлы конфигураций и шаблоны для сервисов, операционных систем и контейнеров.
-   `examples` — примеры использования приложений и библиотек.
-   `sсripts` — скрипты для установки, настройки и других действий с проектом.
-   `tools` — инструменты для поддержки проекта. Могут быть написаны на Go c использованием пакетов проекта.
-   `website` — директория с файлами для веб-сайта проекта.

Это неполный список. Можно создавать директории с другими именами. Старайтесь давать такие имена, которые раскрывали бы назначение директории.

## Дополнительные материалы

-   [Style guideline for Go packages](https://rakyll.org/style-packages/)
-   [Practical Go: Real world advice for writing maintainable Go programs](https://dave.cheney.net/practical-go/presentations/qcon-china.html)
-   [Standard Go Project Layout](https://github.com/golang-standards/project-layout/blob/master/README_ru.md)