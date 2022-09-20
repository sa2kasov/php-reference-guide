# Справочное руководство по PHP

### Содержание

1. [Предыстория](#Предыстория)
2. [Интерпретатор PHP](#Интерпретатор-PHP)
   1. [Связка с веб-сервером Apache](#Связка-с-веб-сервером-Apache)
3. [Синтаксис языка](#Синтаксис-языка)
   1. [PHP–теги](#PHP–теги)
   2. [Комментарии](#Комментарии)
4. [Hello, World!](#Hello,-World!)
5. [Переменные](#Переменные)
   1. [Экранирование переменных](#Экранирование-переменных)
6. [Ошибки](#Ошибки)
7. [Типы данных](#Типы-данных)
   1. [Boolean](#boolean)
   2. [Integer и Float](#Integer-и-Float)
   3. [String](#String)
      1. [Синтаксис heredoc и nowdoc](#Синтаксис-heredoc-и-nowdoc)
      2. [Доступ к символу в строке](#Доступ-к-символу-в-строке)
   4. [NULL](#NULL)
8. [Операторы](#Операторы)
   1. [Арифметические](#Арифметические)
   2. [Строковые](#Строковые)
   3. [Операторы сравнения](#Операторы-сравнения)
   4. [Логические операторы](#Логические-операторы)
9. [Полезные функции](#Полезные-функции)
10. [Управляющие конструкции](#Управляющие-конструкции)
    1. [if](#if)
    2. [if..else..](#ifelse)
    3. [if..elseif..else..](#ifelseifelse)
    4. [Альтернативный синтаксис](#Альтернативный-синтаксис)
    5. [Тернарный оператор](#Тернарный-оператор)
    6. [switch](#switch)
11. [Массивы](#Массивы)
    1. [Нумерованный массив](#Нумерованный-массив)
    2. [Ассоциативный массив](#Ассоциативный-массив)
    3. [Многомерный массив](#Многомерный-массив)
    4. [Работа с массивами](#Работа-с-массивами)
12. [Константы](#Константы)
13. [Циклы](#Циклы)
    1. [for](#for)
    2. [while](#while)
    3. [do..while](#dowhile)
    4. [foreach](#foreach)

## Предыстория

**PHP** – расшифровывается как _PHP: Hypertext Preprocessor_ (рекурсивный акроним), язык общего назначение активно применяемый в веб-разработке. Берёт начало в 1994 году когда датский программист [Расмус Лердорф](https://ru.wikipedia.org/wiki/%D0%9B%D0%B5%D1%80%D0%B4%D0%BE%D1%80%D1%84,_%D0%A0%D0%B0%D1%81%D0%BC%D1%83%D1%81) разработал на языке C набор CGI-скриптов обрабатывающие HTML-документы для учёта посетителей его онлайн-резюме. Первый релиз состоялся 8 июня 1995 году.

Язык подвергся значительной переработке когда в 1997 году два израильских программиста, [Энди Гутманс](https://ru.wikipedia.org/wiki/%D0%93%D1%83%D1%82%D0%BC%D0%B0%D0%BD%D1%81,_%D0%AD%D0%BD%D0%B4%D0%B8) и [Зеев Сураски](https://ru.wikipedia.org/wiki/%D0%A1%D1%83%D1%80%D0%B0%D1%81%D0%BA%D0%B8,_%D0%97%D0%B5%D0%B5%D0%B2), полностью переписали код интерпретатора. Новая версия языка PHP 3.0 была выпущена в июне 1998 года. С тех пор язык привлёк много сторонников, внёсшие вклад над разработкой новых модулей и API. Сегодня язык разрабатывается группой энтузиастов и распространяется под собственной лицензией в рамках проекта с открытым исходным кодом.

## Интерпретатор PHP

Местоположение интерпретатора можно узнать по информации выдаваемой функцией `phpinfo()`. В системе Windows путь к директории, где установлен PHP можно узнать в `Переменные среды` системы, путь к интерпретатору будет в списке переменной среды `Path`.

Стандартный при поставке PHP содержит больше >9000 функций. При разработке нет необходимости в таком количестве функций, поэтому часть стандартных функций вшиты в ядро, а часть функций (расширений) находятся в папке `ext`. В зависимости от необходимости мы можем подключать то или иное расширение.

`php.ini` – главный конфигурационный файл настроек PHP. При установке нужно при необходимости переименовать файл `php.ini-production` или `php.ini-development` в `php.ini`.

Настройки PHP делятся на три группы где их можно изменять:
1. Это непосредственно файл `php.ini`. На shared-хостинге, как правило, не дают прямой доступ к файлу `php.ini`, лишь выносят часть их настроек в панель управления хостингом;
2. `.htaccess` – настройки распространяются ко всем подпапкам, кроме тех, у которых есть свой файл `.htaccess`;
3. Прямо в коде. Например:

```php
<?php php_flag short_open_tag off ?>
```

### Связка с веб-сервером Apache

Чтобы подключить PHP к веб-серверу Apache, нужно указать Apache, где находится файл `php.ini` одним из следующим способом:

1. Прописать путь к файлу php.ini в значение:
  * С помощью директивы `PHPIniDir` указать путь к файлу `php.ini` в главном конфигурационном файле Apache `httpd.conf` (в версиях Apache 2 и выше);
  * Системной переменной `PHPRC`;
  * Ключа реестра `IniFilePath` в ветвях:
    * HKEY_LOCAL_MACHINE\SOFTWARE\PHP\x.y.z\ (5.4.8)
    * HKEY_LOCAL_MACHINE\SOFTWARE\PHP\x.y\ (5.4.8)
    * HKEY_LOCAL_MACHINE\SOFTWARE\PHP\x\ (5.4.8)
    * HKEY_LOCAL_MACHINE\SOFTWARE\PHP\

2. Скопировать файл `php.ini` в директорию:
  * PHP;
  * Сервера;
  * ОС Windows (например, C:\Windows).

Узнать какой именно файл `php.ini` зачитывается можно с помощью той же функцией `phpinfo()` в строке `Loaded Configuration File`.

## Синтаксис языка

### PHP–теги

Все, что находится между открывающими и закрывающими php-тегами – есть код на PHP. Каждая инструкция заканчивается точкой с запятой.

**Рекомендуется к использованию:**

```php
<?php инструкция; ?>
```

**Короткая запись**

Для этого тега параметр `short_open_tag` в `php.ini` должен быть установлен как `on`. Такая конструкция может быть нежелательной из-за того, что декларация XML также начинается с `<?` и заканчивается `?>`:

```php
<? код PHP ?>
```

**Вместо вопросов проценты**

В свое время, чтобы переманить на свою сторону программистов на ASP.NET были введены такие открывающиеся/закрывающие теги PHP. Для этого параметр `asp_tags` должен быть включен:

```php
<% код PHP %>
```

### Комментарии

В классическом программировании принято чтобы комментарии составляли 30% от кода. Однострочные комментарии начинаются с двумя слэшами `//`. Блочные комментарии заключаются `/*` и `*/`.

```php
<?php 
# однострочный комментарий
// однострочный комментарий
?>

<?php
/*
это блочный комментарий
*/
?>
```

## Hello, World!

За вывод строковых данных на экран в PHP используется функция `echo`, которая выводит одну и более строк. Языковая конструкция `print` также печатает данные на экран. В отличие от `echo` она умеет принимать только один параметр и всегда возвращает 1.

```php
<?php
echo "Привет, это просто текст";
print "Привет, это просто текст";
print("так тоже можно");

// Здесь четыре раза вызывается echo
echo '<h1>', 'Hello, ', 'world!', '</h1>';

// Короткая запись (сокращенное echo)
// Для этого должен быть включён флаг short_open_tag
<?='Hello, world!'?>
?>
```

## Переменные

Переменная — именованная ячейка памяти.
* Все имена переменных должны начинаться со знака доллара `$`;
* Первым символом после `$` должна быть буква или символ подчеркивания. Далее, в имени переменной могут присутствовать буквы, цифры и символ подчеркивания;
* Имена переменных чувствительны к регистру.

Есть несколько подходов к написанию переменных. В C (Си) подобных языках переменные пишут с маленькой буквы, в Basic с заглавной. Нужно определить свой стиль написания переменной. Если переменная состоит из двух слов, то можно записать так: `user_name` или методом "верблюжьей нотацией" `userName` (`UserName`). Последняя чаще может встречаться в крупных проектах в таких, как WordPress.

**Присвоение значения переменной и её удаление**

```php
<?php
// Переменной $var присваивается значение John
$var = 'John';

// Переменной $num присваивается значение 99
$num = 99;

// Переменной $num присваивается результат выражения, т.е. 10
$num = 5 + 5;
?>
```

После завершения конструкции PHP значения переменных стираются из памяти, но бывают ситуации, когда переменные нужно удалить до того, как закончится код.

```php
<?php
$user = "John";
echo $user;

// Удаление из памяти переменную $user
unset($user);

// Уведомление (PHP Notice) о том, что переменная неопределена
echo $user;
?>
```

### Экранирование переменных

Имена переменных можно помещать в строки (тип данных String рассматривается в разделе [Типы данных](#String)). В строках заключенные одинарными кавычками переменные отображаются как простой текст. Однако в двойных кавычках переменная подставит своё значение.

```php
<?php
$beer = 'Heineken';

// Выведет переменную beer,
// т.к. апостроф не допустимый символ в именах переменных
echo "$beer's taste is great<br>";

// Переменной $beers не существует
echo "He drank some $beers<br>";
?>
```

Если имя переменной сливается в строке с другими символами, и интерпретатор может допустить лишние символы как часть имени переменной, то такую переменную следует экранировать заключив её имя в конструкции `${name}` или `{$name}`.

```php
// Экранирование переменных одним из двух способов
echo "He drank some ${beer}s<br>"; // первый способ
echo "He drank some {$beer}s<br>"; // второй способ
```

## Ошибки

* Если мы не видим ошибки на строке который указывает нам браузер, то ошибку следует искать строчкой выше;
* Когда у нас выпадает много ошибок, нужно искать и исправлять ошибки сверху вниз, а не снизу вверх.

У PHP есть четыре уровня ошибок (предупреждений):
1. **PARSE** – ошибка при парсинге. PHP не сразу выполняет полученный код, а сперва проверяет на предмет синтаксических ошибок;
   Если синтаксических ошибок не обнаружено, то код начинает выполняться...
2. **FATAL ERROR** – в этом случае нам выведется ошибка и выполнение кода прекратится;
3. **WARNING** – предупреждение. Выведет ошибку, но выполнение кода продолжится;
4. **NOTICE** – уведомление.

Мы можем запретить выводить показ об ошибке в браузере выполнив `error_reporting(0);`. По умолчанию все константы уровней ошибок (кроме NOTICE) будут выводиться в браузере. На практике сначала проверяют код на предмет серьезных ошибок, и только потом если таких ошибок не обнаружено, включают `error_reporting(E_ALL)`.

## Типы данных

PHP поддерживает 10 простых типов:

* Четыре скалярных типа:
  * boolean (логический)
  * integer (целые числа)
  * float (вещественные (плавающие) числа)
  * string (строковый тип)

* Два смешанных типа:
  * array (массив)
  * object (объект)
  * callable
  * iterable

* Два специальных типа:
  * resource
  * NULL

В этом разделе рассмотрим только скалярные типы и специальный тип `NULL`. 

### Boolean

Простейший тип, выражающий истинность значения. Это может быть либо `true` или `false`.
При преобразовании в логический тип, следующие значения рассматриваются как `false`:
* само значение `false`;
* Целое 0 (ноль);
* Вещественное число (с плавающей точкой) 0.0 (ноль);
* Пустая строка и строка `"0"`;
* Пустой массив;
* Специальный тип `NULL` (включая неустановленные переменные).

Все остальные значения рассматриваются как `true`.

### Integer и Float

```php
<?php
// Целые числа
$int = 123; // Целое десятичное число
$int = -23; // Целое отрицательное число
$int = 0123; // Восьмиричное число (начинается с 0)
$int = 0x1A; // Шестнадцатиричное число

// Вещественные числа
$flt = 1.23; // Дробное число
$flt = 1.2e3; // экспоненциальная запись
$flt = 7E-10;
?>
```

### String

Строки заключаются в двойные и одинарные кавычки, а также доступен синтаксис `heredoc` рассматриваемый дальше.

```php
<?php
$str = "Текстовая строка"; // в двойных кавычках
echo $str;
?>
```

В строках с двойными кавычками допустимо использовать специальные символы, некоторые из них:

- \n - перевод на новую строку;
- \r - возврат каретки;
- \t - горизонтальная табуляция;
- \\ - обратный слеш;
- \$ - знак доллара;
- \" - двойная кавычка;

```php
<?php
$str = 'Текстовая строка'; // в одинарных кавычках
echo 'Переменная $str - ' . $str; // -> Переменная $str - Текстовая строка
?>
```

Разница между двойными и одинарными кавычками в том, что в двойных переменные и спец. символы обрабатываются, а в одинарных нет.

### Синтаксис heredoc и nowdoc

Конструкция **heredoc** и **nowdoc** начинаются с тремя знаками `<<<` после которого следует идентификатор, а затем перевод строки. После этого идёт сама строка, а потом этот же идентификатор, закрывающий вставку.

Закрывающий идентификатор может иметь отступ в виде пробела или табуляции. В этом случае отступ будет удалён из всех строк в строке документа.

```php
<?php
echo <<<LABEL
Пример строки, охватывающей несколько строк, с использованием
heredoc-синтаксиса.

Это очень похоже на использование HTML-тега <PRE>,
т.е. сохраняются пробелы, отступы, переходы на новую строку,
табуляции, также сюда можно подставлять переменные, использовать
специальные символы.
LABEL;
?>
```

**Определение строк с помощью синтаксиса nowdoc**

**nowdoc** — это то же самое для строк в одинарных кавычках, что и heredoc для строк в двойных кавычках. **nowdoc** похож на **heredoc**, но внутри него не осуществляется никаких подстановок. Эта конструкция идеальна для внедрения PHP-кода или других больших блоков текста без необходимости его экранирования.

**nowdoc** указывается той же последовательностью `<<<`, что используется в **heredoc**, но последующий за ней идентификатор заключается в одинарные кавычки, например, `<<<'EOT'`.

```php
<?php
echo <<<'LABEL'
строка nowdoc
LABEL;
?>
```

Некоторые правила касающиеся типа string **heredoc** и **nowdoc**:

* Строка с открывающим идентификатором не содержит после него никаких других символов, включая пробел;
* Закрывающий идентификатор до версии PHP 7.3.0 должен начинаться с первой позиции строки;
* Строка с закрывающим идентификатором не содержит других символов (включая пробел), за исключением точки с запятой.

### Доступ к символу в строке

Мы можем получить доступ к любому символу в строке объявленной в переменной переменной `$str` передав номер позиции символа в квадратных `[]` или фигурных `{}` скобках. Отсчёт позиции символов начинается с нуля.

```php
<?php
$str = "Hello, world!";

// Получение первого символа строки
$first = $str{0}; // Возвращает позицию первого символа, т.е. H

// То же самое можно сделать используя квадратные скобки
$first = $str[0]; // Возвращает позицию первого символа, т.е. H

// Получение третьего символа строки
$third = $str{2}; // Возвращает l

// Получение последнего символа строки
$last = strlen($str); // Возвращает количество символов (отсчет с 1)
$sym = $str{$last - 1}; // Возвращает ! знак
print "$sym"; // Печатает ! знак
?>
```

### NULL

Переменная считается NULL если:

* Ей была присвоена константа `NULL`;
* Ей ещё не было присвоено какое-либо значение;
* Она была удалена с помощью `unset()`.

## Операторы

Операторы принимают одно или несколько значений и вычисляют на их основе новое значение. Под вычислением может происходить сравнение двух значений, их арифметическое вычисление, присваивание и т.д.

Операторы бывают _унарные_ – принимают только одно значение, например `!` (логическое отрицание), `++` (инкремент) и др. и _бинарные_ – принимают два значения (большинство операторов именно такие). Также существует один [_тернарный оператор_](#Тернарный-оператор), принимающий три значения.

### Арифметические

```php
<?php
$a + $b; // Сумма $a и $b
$a - $b; // Разность $a и $b
$a * $b; // Произведение $a и $b
$a / $b; // Частное от деления $a на $b
$a % $b; // Целочисленный остаток от деления $a на $b
$a += $b; // Тоже, что и $a = $a + $b.

// Остальные операторы работают аналогично
$a = '5Word';
$b = $a * 10; // От переменной $a возмется 5 и результат будет 50

$a = 'Строка';
$b = $a * 10; // Переменная $a приведется к нулю и результат будет 0
?>
```

### Строковые

```php
<?php
$word1 = "Привет!";
$word2 = $word1 . " Мир"; // Конкатенация (возвращает "Привет! Мир")

$word1 .= " Мир"; // Конкатенация сокращенная (возвращает "Привет! Мир")

$word3 = "Я";
$word4 = "люблю";
print $word3 . " " . word4 . " " . "кофе!";
echo $word3, ' ', $word4, ' ', 'чай!';
?>
```

### Операторы сравнения

```php
<?php
$a == $b // -> true если $a равно $b
$a === $b // -> true если тип операндов совпадает и $a равно $b
$a != $b // -> true если $a не равно $b
$a !== $b // -> true если типы не совпадают и $a не равно $b
$a > $b // -> true если $a строго больше $b
$a < $b // -> true если $a строго меньше $b
$a >= $b // -> true если $a больше или равно $b
$a <= $b // -> true если $a меньше или равно $b
?>
```

### Логические операторы

```php
<?php
$a and $b // -> true если $a и $b true
$a or $b // -> true если $a или $b true
!$a // -> true если $a не true

// Сначала вычислится сравнение $a и $b,
// и только потом их результат сравнится с $c
$a and $b and $c

// Сначала вычислится сравнение $b и $c,
// и только потом их результат сравнится с $a
$a and ($b and $c) эквивалентно $a and $b && $c

// Сначала вычислится сравнение $b или $c,
// и только потом их результат сравнится с $a
$a and ($b or $c) эквивалентно $a and $b || $c
?>
```

Операторы `&&` (логическое умножение), `||` (логическое сложение) приоритетнее операторов `and` и `or`.

## Полезные функции

- `isset(имя_переменной)` – возвращает `true`, если существует переменная или `false`, если переменная не определена или ей присвоено `NULL` (что в принципе одно и то же);

- `empty(имя_переменной)` – возвращает `true`, если значение переменной пустая строка `""`, `NULL`, переменная не определена ($x), пустой массив ($x = array()), значение `false` ($x = false), значение `0` ($x = 0;) или ($x = "0"). Возвращает `false`, если значение переменной `true` ($x = true), значение `>` или `<` 0;

- `gettype(имя_переменной)` – возвращает тип переменной (boolean, string, integer, double(float), NULL). Например:

```php
<?php
$variable = "100darov";
echo gettype($variable); // Возвращает string
?>
```

- `settype(имя_переменной, "тип")` – конвертирует переменную в другой тип. Например:

```php
<?php
$variable = "100darov";
settype($variable, "integer"); // Возвращает integer
?>
```

- Временное приведение к типу

Если мы хотим вывести только 100, но также хотим чтобы значение `"100darov"` в переменной `$variable` осталось — нужно написать в скобках нужный тип данных перед именем переменной без пробелов:

```php
<?php
$variable = "100darov";
echo (integer)$variable; // -> 100
?>
```

## Управляющие конструкции

Основные конструкции PHP это _условные операторы_ (if, else), _оператор выбора_ (switch) и _циклы_ (for, foreach, while, do..while). Внутри конструкции `switch` и некоторых циклов могут быть использованы ключевые слова (return, break, continue).

### if

Если условие в скобках истинно, но выполнится инструкция.

```php
<?php
$var = true;
if ($var) echo "Инструкция выполнится, в $var истинное значение";
?>
```

Если необходимо выполнить несколько инструкций, то заключаем их в операторные скобки.

```php
if (условие) {
  инструкция 1;
  инструкция 2;
}
?>
```

### if..else..

Если условие в `if` окажется истинным, то выполнятся только инструкции 1 и 2, иначе выполняются инструкции 3 и 4.

```php
<?php
if (условие) {
  инструкция 1;
  инструкция 2;
} else {
  инструкция 3;
  инструкция 4;
}
?>
```

### if..elseif..else..

Если `условие 1` истинно, то выполнится только `инструкция 1`, все остальные `elseif` и `else` интерпретатор проверять не станет. Аналогично и с `условием 2`, во всех остальных случаях выполнится `инструкция 3` в блоке `else`.

`Инструкция 4` выполнится в любом случае, сразу после разрешения конструкции выше.

```php
<?php
if (условие 1) {
  инструкция 1;
}
elseif (условие 2) {
  инструкция 2;
} else {
  инструкция 3;
}

инструкция 4;
?>
```

### Альтернативный синтаксис

Закрытие блока `?>`, это еще не есть закрытие кода. Очень легко запутаться в фигурных скобках, поэтому для удобства PHP предлагает альтернативный синтаксис для некоторых его управляющих структур, а именно: `if`, `while`, `for`, `foreach` и `switch`.

```php
<?php if (условие 1): ?>
  код условия 1
<?php elseif (условие 2): ?>
  код условия 2
<?php else: ?>
  код блока else
<?php endif; ?>
?>
```

### Тернарный оператор

Тернарный оператор это то же условие `if..else`, но с более коротким синтаксисом. Сначала проверяется условие в скобках, если оно истинно, то выполняется инструкция после знака `?`, иначе после знака `:`.

Скобки вокруг условия факультативны, но предпочтительней их ставить для лучшей читаемости кода.

```php
<?php
echo ($a > 1) ? "a больше единицы" : "a меньше единицы";
$b = ($a > 1) ? "One" : "Zero"; // Значение присвоится переменной $b
?>
```

### switch

Оператор `switch` можно представить как ещё одну разновидность оператора `if..elseif..else`. В случаях, когда необходимо сравнивать одну и ту же переменную (или выражение) со множеством значений.

Конструкция `switch/case` использует нестрогое сравнение `==`.

Оператор `continue` прерывает все последующие итерации. Следующая за `continue` инструкция и далее уже не будут выполнены. Может также быть использована в циклических структурах и выполнять там ту же функцию.

`break` прерывает выполнение текущей структуры `switch` и выходит за операторные скобки. Может также применяться в таких циклических конструкциях как `for`, `foreach`, `while`, `do..while`.

`continue` и `break` принимают числовой параметр, сообщающей интерпретатору какое кол-во вложенных структур надо прервать. Поведение работы этих операторов в циклических структурах описано далее на примере работы цикла [**while**](#while).

В случае если ни один из вариантов не подошел, то будет исполнено то, что в блоке `default`.

```php
<?php
switch (выражение) {
  case 0: инструкция 1;
  case 1: инструкция 2;
  case 2:
    инструкция 3;
    continue;
  case 3: инструкция 4;
  case 4:
    инструкция 5;
    break;
  case 5: инструкция 6;
  case 6: инструкция 7;
  default:
    инструкция 8;
}
?>
```

**Пример использования `switch`**

К примеру необходимо вычислить значение директивы `post_max_size` в байтах. Значение любой директивы в PHP можно получить/установить с помощью функций:

* `ini_get('имя_директивы')` — данная функция всегда возвращает значение в строковом типе;
* `ini_set('имя_директивы', "on | of | значение")` — включение/выключение/изменение директивы Runtime (прямо в коде).

Допустим мы имеем код со следующей реализацией:

```php
<?php
$result = ini_get("post_max_size"); // -> 8M
$letter = $result{strlen($result) - 1}; // -> M
$size = (integer)$result; // -> 8

switch ($letter) {
  case "G": $size = $size * 1024 * 1024 * 1024; break;
  case "M": $size = $size * 1024 * 1024; break;
  case "K": $size = $size * 1024; break;
  default: $size;
}

echo "POST_MAX_SIZE = " . $size . " bytes";
?>
```

Вот как выглядит код после оптимизации. Пример демонстрирует случай, когда такое использование `switch` особенно оправдано.

```php
<?php
$result = ini_get("post_max_size"); // -> 8M
$letter = $result{strlen($result) - 1}; // -> M
$size = (int)$result; // -> 8

// Альтернативный фигурным скобкам синтаксис
switch ($letter):
	case "G": $size *= 1024; // -> 8 589 934 592
	case "M": $size *= 1024; // -> 8 388 608
	case "K": $size *= 1024; // -> 8 192
endswitch;
?>
```

## Массивы

_Массив_ — упорядоченный набор данных, где каждый элемент массива имеет свой ключ и значение. Массив может быть создан языковой конструкцией `array()` или с помощью короткого синтаксиса `[]`. Ключи массива могут быть либо целочисленным типом [Integer](#Integer-и-Float), либо [String](#String). Значение же может быть любым типом.

### Нумерованный массив

_Нумерованные массивы_ — это массивы, у которых ключи являются числа.

```php
<?php
$arr = [0 => '1st', 1 => '2nd', 3 => '3rd'];

// Значение "4th" теперь будет имет порядковый номер 10
$arr[10] = '4th';
?>
```

### Ассоциативный массив

_Ассоциативный массив_ — это когда вместо индексов (номеров) назначают строковые осмысленные имена.

```php
<?php
$user = Array(
  "Jane",
  "Doe",
  20,
  // Значение "guest" теперь будет имет порядковый номер (индекс) 55
  55 => "guest",
  true
);
?>
```

У ниже приведённых двух элементов массива будет одинаковая структура, хоть они и были созданы разными способами. 

```php
<?php
$cars['tesla'] = Array(
  'model' => "Model S Plaid",
  'speed' => 321,
  'doors' => 4,
  'year' => "2022"
);

$cars['mercedes-benz']['model'] = 'Mercedes-Benz EQC';
$cars['mercedes-benz']['speed'] = 180;
$cars['mercedes-benz']['doors'] = 4;
$cars['mercedes-benz']['year'] = "2022";
?>
```

### Многомерный массив

_Многомерный массив_ — массив, где элементы в свою очередь могут сами являться массивами и содержать в себе сколько угодно вложенных массивов. Пример многомерного массива:

```php
<?php
$contents = array(
"Введение",
"Часть I" => array(
  "Глава I" => array(
    "Глава 1.1",
    "Глава 1.2",
    "Глава 1.3",
    ),
  "Глава II",
  "Глава III",
  ),
"Часть II",
"Часть III" => array(
  "Глава I" => array(
    "Глава 1.1",
    "Глава 1.2",
    "Глава 1.3",
    ),
  "Глава II",
  "Глава III" => array(
    "Глава 1.1",
    "Глава 1.2",
    "Глава 1.3",
    ),
  "Глава IV",
  ),
"Часть IV",
"Заключение",
);
?>
```

### Работа с массивами

Объявлять массив предпочтительней с помощью языковой конструкцией `array()`, но добавлять новые ячейки в уже имеющийся массив удобней с помощью квадратных скобок `$myArray[] = 'добавление'`.

**Печать массива**

Можно просмотреть индексы массива и соответствующие им значения с помощью функции `print_r()`. Для сохранения форматирования вывод был обрамлён тегами `<pre>`.

```php
echo '<pre>', print_r($user), '</pre>';
```

Функция `var_dump()` также печатает массив, в отличие от предыдущей данные массива печатаются с дополнительной информацией — с указанием длины массива, индекса, тип значения и само значение, а также длину строки, если значение строковое. `var_dump()` также можно применить и к обычной переменной и в случаях, когда необходимо более подробно посмотреть массив.

```php
echo "<pre>", var_dump($cars), "</pre>";

// Результат выполнения
array(1) {
  ["tesla"]=>
  array(4) {
    ["model"]=>
    string(13) "Model S Plaid"
    ["speed"]=>
    int(321)
    ["doors"]=>
    int(4)
    ["year"]=>
    string(4) "2022"
}
```

**Подсчёт элементов массива**

Подсчет количества элементов массива или количества свойств объекта
выполняет функция `count($myArray)`.

Если в качестве второго параметра передать `true`, т.е сделать так `count(myArray, 1)`, то функция подсчитает все вложенные элементы, если это многомерный массив.

**Встраивание массива в строки**

Вывести ячейки массива внутри двойных кавычек можно двумя способами:

* Экранировать переменные:

```php
<?php
echo "Меня зовут {$user['name']}! Мне {$user['age']} лет";
?>
```

* Обращаться к ячейкам массива без кавычек:

```php
<?php
echo "Меня зовут $user[name]! Мне $user[age] лет";
?>
```

**Перемещение указателя массива**

Можно передвигать указатель массива (вверх-вниз) с помощью специальных функций:

```php
<?php
// Исходный маассив
$menu['Блюдо']['Суп1'] = "Борщ";
$menu['Блюдо']['Суп2'] = "Щи";
$menu['Блюдо']['Суп3'] = "Рассольник";
$menu['Блюдо']['Салат1'] = "Венегрет";
$menu['Блюдо']['Салат2'] = "Оливье";
$menu['Блюдо']['Салат3'] = "Под шубой";

// Возвратить текущий элемент массива
current($menu['Блюдо']); // -> Борщ

// Переход на следующую позицию
next($menu['Блюдо']); // -> Щи

// Переход на предыдущую позицию
prev($menu['Блюдо']); // -> Борщ

// Переход к последнему элементу
end($menu['Блюдо']); // -> Под шубой

// Возвращает ключ из ассоциативного массива
key($menu['Блюдо']); // -> Салат3
?>
```

## Константы

_Константы_ — те же самые переменные, но только значения в них неизменны.

* У констант нет приставки в виде знака доллара;
* Константы можно определить только с помощью функции `define()`, а не присваиванием значения;
* Константы доступны в любом месте, без учета области видимости;
* Константы не могут быть переопределены или аннулированы после первоначального объявления;
* Константы не встраивают в строки с двойными кавычками, их нужно конкатенировать.

Константы задаются следующей функцией:

```php
<?php define('имя_константы', значение); ?>
```

Константу можно проверить следующей функцией:

```php
<?php
defined("имя_константы"); // -> true, если такая константа уже есть

// нечувствительная к регистру константа
define("имя_константы", значение, true);
?>
```

Передав `true` третьим параметром мы разрешаем константе быть регистро-независимой (не рекомендуется).

Имена констант принято писать большими буквами (но допускается и строчными). Единожды созданная константа существует до конца кода. Константе нельзя заново присвоить какое-нибудь значение, её нельзя удалить (с помощью функции `unset()`).

Константы обычно заводятся по двум причинам:

1. Удобней помнить имя константы. Например константы `E_ALL`, `E_ERROR` и др. за такими константами на самом деле стоит определенное значение (число). И гораздо удобней запомнить имя константы, чем то число, которое стоит за ней;
2. В процессе работы мы заводим переменную значение которой впоследствии можно ненамеренно переназначить. С использованием констант эта проблема отпадает, т.к. её значение всегда остаётся неизменным.

## Циклы

PHP поддерживает несколько различных циклических структур управления.

### for

Синтаксис цикла `for`:

```php
<?php
for (часть A; часть B; часть C) {
  инструкция 1;
  инструкция 2;
  ...
}
?>
```

Параметры цикла `for` (в круглых скобках) можно разделить на три части, которые в свою очередь разделяются `;`, т.е. в данном случае точка с запятой выступают как разделитель, а не как окончание выражения или инструкции.

* Выражения, находящиеся в `части A`, PHP просто выполняет, и как правило, там инициализируется счетчик (т.е. мы назначаем какую-то переменную и говорим что она будет счетчик);
* `Часть B` (или так называемый встроенный `if`), здесь проверяется истинность, и в случае, если там `true`, то PHP заходит в тело цикла;
* Содержимое `части C` PHP, как и в случае с `частью A` просто выполняет и как правило, там мы изменяем наш счетчик.

В каждом из этих трех частей мы можем использовать не одно выражение, а сколько угодно, но в этом случае поскольку точка с запятой служит разделителем частей в конструкции for, разделять выражения следует запятой.

Когда интерпретатор PHP встречает цикл `for`, первое что он делает — заходит в `часть A` и делает все что там написано. Сюда он попадает только один раз. Потом он перемещается в `часть B` и если там `true`, то PHP выполняет тело цикла. Затем выполняется `часть C`. Таким образом, далее цикл работает как по треугольнику: `часть B` => тело цикла => `часть C`, до тех пор, пока условие в `части B` не окажется ложным.

Например: вывести нечетные числа от 0 до 50.

```php
<?php
$str = "Привет! Мир";
for($j=0, $cnt = strlen($str); $j < $cnt; $j++) {
  echo $str{$j}, "<br>";
}
?>
```

В цикле `for` любую из частей можно опустить. Пример классического бесконечного цикла:

```php
<?php
for(;;) {
  print $a . "<br>";
}
?>
```

### while

Синтаксис:

```php
<?php
while(условие) {
  инструкция 1;
  инструкция 2;
}
?>
```

Если условие истинно, то выполняется тело цикла. Цикл `for` и `while` взаимозаменяемы. Если мы знаем начальную и конечную точку (от скольки и до скольки), то удобнее использовать цикл `for`. Но бывает ситуация, когда мы не знаем конечной точки.

В теле цикла `while` можно использовать оператор `break`, который прерывает цикл и выйдет за его пределы.

```php
<?php
$i = 1;

// Скрипт последовательно напечатает 1 2 3 4 5
// из-за условия внутри тела цикла
while($ < 10) {
  $i++;
  print $i . "<br>";
  if ($i == 5)
    break;
} 
?>
```

Оператор `continue` в цикле `while` прерывает итерацию (переходит обратно к условию):

```php
<?php
$i = 0;

// Печатает числа от 1 до 10 пропустив число 5
while($i < 10) {
  $i++;
  if ($i == 5)
    continue;
  print ($i . "<br>");
}
?>
```

**Операторы `break` и `continue` во вложенных циклах**

По умолчанию `break` и `continue` выходят на один уровень вложенности, если необходимо чтобы `break` или `continue` участвовал и во внешних циклах, надо указать через пробел число соответствующее уровню:

```php
<?php
$levelOne = 0;

// Печатает таблицу умножения от 1 до 10
while ($levelOne < 10) {
  $levelOne++;
  $levelTwo = 0;
  while ($levelTwo < 10) {
    $levelTwo++;
    if ($levelOne == 2)
      break 2; // или continue 2
    echo "| ", $levelOne * $levelTwo, " ";
  }
}
?>
```

Если во втором вложенном цикле `while` используется `break`, то скрипт напечатает таблицу умножения только для числа 1 (`1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10`) и прервет внешний цикл выйдя из него насовсем.

Если же будет использоваться `continue`, то скрипт прервёт лишь итерации для числа 2, но продолжит выполнение для остальных чисел (`1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 3 | 6 | 9 | 12 | 15 | 18 | 21 | 24 | 27 | 30` ...и так далее)

    ❗️ Значение оператора `break` или `continue` не должны
    превышать уровень вложенности структуры.

### do..while

Синтаксис:

```php
<?php
do {
  инструкция 1;
  инструкция 2;
} while (условие);
?>
```

Отличие от цикла `while` в том, что если условие изначально ложно, тело цикла выполнится хотя бы один раз. Пример с таблицей умножения в цикле `while` можно переписать так:

```php
<?php
$levelOne = 0;

do {
  ++$levelOne;
  $levelTwo = 0;
  do {
    $levelTwo++;
    if ($levelOne == 2)
        continue 2;
    echo '• ', $levelOne * $levelTwo, ' ';
  } while($levelTwo < 10);
} while($levelOne < 10);
?>
```

### foreach

Конструкция `foreach` (для каждого) дает простой способ перебора массивов. `foreach` работает только с массивами и объектами, и будет генерировать ошибку при попытке использования с переменными других типов или неинициализированными переменными. Чтобы вывести только значения элементов массива, применим короткий синтаксис:

```php
<?php
$user = array(
  "name" => "Jane",
  "last-name" => "Doe",
  "age" => 35,
  "height" => 175,
  "weight" => 55,
  "hair-color" => 'brown'
);

foreach($user as $value) {
  echo $value, "<br>";
}
?>
```

Вторая форма делает то же самое, за исключением того, что ключ текущего элемента будет присвоен переменной `$k` соответствующей каждому значению:

```php
<?php
foreach($myArray as $k => $v) {
  echo $k . " — " . $v . "<br>";
  if (is_string($v))
    $v = '';
}
?>
```

В выражении `$v = ''` на оригинальный массив эти изменения не повлияют, т.к. в переменную `$v` передается копия значения.

Если всё же необходимо, чтобы изменения затрагивали данные исходного массива который перебираем, то перед переменной используется `&`, тогда обращение будет происходить по ссылке на оригинальный массив.

```php
<?php
foreach($array as &$v)
  if (is_string($v))
    $v = '';
?>
```

