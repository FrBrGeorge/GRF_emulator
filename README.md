# GRF_emulator
GRF emulator - интерпретатор языка, который основан на частично рекурсивных
функциях и только на них.

# Запуск
Запуск производится командами
```
pipenv shell
python -m grfemulator
```

# Синтаксис языка

## Функции
### Тождественный ноль
`o` - одноместный тождественный 0, т.е. $o(x) = 0$.

Пример:
```
o
```
### Функция следования
`s` - функция следования, т.е. $s(x) = x + 1$.

Пример:
```
s
```
### Функция выбора
`I^<n>_<m>` - функция выбора с параметрами $n$ и $m$, т.е.
$I^n_m(x_1, \dots, x_n) = x_m$.

Пример:
```
I^5_2
```
Который обозначает:
$$I^5_2(x_1, \dots, x_5) = x_2$$
### Константные функции
- `0^<n>` - n-местный тождественный 0, т.е. $o^n(x_1, \dots, x_n)$, 
то есть функция от $n$ параметров, возвращающая $0$ .
- `<const>^<n>` - n-местная тождественная константа,
т.е. $s(s(...s(o^n(x_1, \dots, x_n))...))$,
функция от $n$ параметров, возвращающая число $const$.

Примеры:
```
0^3
42^1
150^123
```
Которые обозначают функции
$o(x_1, x_2, x_3)=0$, $42(x_1)=42$, $150(x_1, \dots, x_{123})=150$

## Операции над функциями
### Суперпозиция
`F {A, B, C, ..., Z}` - суперпозиция функции $F(x_a, \dots, x_z)$ и функций $A(x_1, \dots, x_k), \dots Z(x_1, \dots, x_k)$ для любого фиксированного $k$.

Пример. Предположим, что `D`, `E`, и `F` — двухместные функции, а `U` — трёхместная. Тогда
```
U {D, E, F}
```
обозначает двухместную функцию (назовём её `S`):

$$U(D(x_1, x_2), E(x_1, x_2), F(x_1, x_2)) = S(x_1, x_2)$$

### Примитивная рекурсия
`F <- G` - примитивная рекурсия «от $F(x_1, \dots, x_n)$ по $G(x_1, \dots, x_n, y, z)$»; арность `G` должна быть на 2 больше арности `F`. Результат примитивной рекурсии — функция `H` арности на 1 больше, чем арность `G` с такими свойствами:
- $H(x_1, \dots, x_n, 0) = F(x_1, \dots, x_n)$
- …
- $H(x_1, \dots, x_n, y+1) = G(x_1, \dots, x_n, y, H(x_1, \dots, x_n, y))$

Пример. Предположим, что `A` — двухместная функция. Тогда `B` — четырёхместная, и
```
A <- B
```
обозначает трёхместную функцию (назовём её `R`):
- $R(a, b, 0) = A(a, b)$
- $R(a, b, 1) = B(a, b, 0, R(a, b, 0)) = B(a, b, 0, A(a, b))$
- $R(a, b, 2) = B(a, b, 1, R(a, b, 1)) = B(a, b, 0, B(a, b, 0, A(a, b)))$
- …
- $R(a, b, n) = B(a, b, n-1, R(a, b, n-1)) = B(a, b, 0, B(\dots, B(a, b, 0, A(a, b)), \dots))$

### Минимизация
`? F` - минимизация $\mu(F)$.

**TODO** дописать определение

## Создание новых функций
Возможно создание пользовательских функций с именами, которые возможны по
правилам языка Си.

**TODO** я бы запретил подчёркивания, ибо они имеют синтаксический смысл для `I^m_n`.

```
F = s;
G = I^5_1;
```
Которые обозначают функции
$$F(x) = s(x)$$
$$G(x_1, x_2, x_3, x_4, x_5) = I^5_1(x_1, x_2, x_3, x_4, x_5)$$
## Вызов функций
Вызов функций производится в отдельном окне вызова функций, которое 
по умолчанию расположено в правом верхнем углу.
Синтаксис вызова функций:
```
F(1, 2, 3, 10)
```
## Комментарии
Поддерживаются комментарии в формате Python и Си.
```
# Это комментарий
/*
Это многострочный комментарий
Это многострочный комментарий
Это многострочный комментарий
*/
```
