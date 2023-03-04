#basics
#int

# int
_class_ `int`(_x_, _base=10_)


-   **int**([object], [основание системы счисления]) - преобразование к целому числу в десятичной системе счисления. По умолчанию система счисления десятичная, но можно задать любое основание от 2 до 36 включительно.
-   **bin**(x) - преобразование целого числа в двоичную строку.
-   **hex**(х) - преобразование целого числа в шестнадцатеричную строку.
-   **oct**(х) - преобразование целого числа в восьмеричную строку.
К классу `int` (от англ. _integer_ — целое число) относятся все положительные и отрицательные числа без дробной части; ноль — это тоже `int`.

Как и в обычной математике, числа используются в арифметических операциях и для описания количественных свойств объектов: например, если у осьминога семь ног — мы не планируем их ни с чем складывать, но уже кое-что знаем о количественных свойствах этого осьминога и о его тяжёлой судьбе.

Для лучшей читаемости целые числа в Python можно записывать, разделяя разряды нижним подчёркиванием: 432246123 ⇒ 432_246_123

Переменные типа `int`, как и переменные других типов, не требуют отдельного объявления: Python автоматически присвоит переменной нужный тип при операции присваивания:

**Основные операции**

Operation | Result | Full documentation
------------| ------------ | ------------
x + y | sum of _x_ and _y_ | - 
x - y | difference of _x_ and _y_ | -
x * y | product of _x_ and _y_ | -
x / y | quotient of _x_ and _y_ | -
x // y | floored quotient of _x_ and _y_| -
x % y | remainder of `x / y` | -
-x |_x_ negated | -
+x | _x_ unchanged | -
abs(x) | absolute value or magnitude of _x_ | [`abs()`](https://docs.python.org/3/library/functions.html#abs)
int(x) | _x_ converted to integer |  [`int()`](https://docs.python.org/3/library/functions.html#int "int")
float(x) | _x_ converted to floating point | [`float()`](https://docs.python.org/3/library/functions.html#float "float")
complex(re, im) | a complex number with real part _re_,imaginary part _im_. _im_ defaults to zero. | [`complex()`](https://docs.python.org/3/library/functions.html#complex "complex")
c.conjugate() | conjugate of the complex number _c_ | -
divmod(x, y) | the pair `(x // y, x % y)` | [`divmod()`](https://docs.python.org/3/library/functions.html#divmod "divmod")
pow(x, y) | pow | -
x ** y | pow | -
