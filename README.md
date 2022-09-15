# Справочное руководство по PHP

### Содержание

1. [Предыстория](#Предыстория)
2. [Интерпретатор PHP](#Интерпретатор-PHP)
   1. [Связка с веб-сервером Apache](#Связка-с-веб–сервером-Apache)
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

## Предыстория

**PHP** – расшифровывается как _PHP: Hypertext Preprocessor_ (рекурсивный акроним), язык общего назначение активно применяемый в веб-разработке. Берёт начало в 1994 году когда датский программист [Расмус Лердорф](https://ru.wikipedia.org/wiki/%D0%9B%D0%B5%D1%80%D0%B4%D0%BE%D1%80%D1%84,_%D0%A0%D0%B0%D1%81%D0%BC%D1%83%D1%81) разработал на языке C набор CGI-скриптов обрабатывающие HTML-документы для учёта посетителей его онлайн-резюме. Первый релиз состоялся 8 июня 1995 году.

Язык подвергся значительной переработке когда в 1997 году два израильских программиста, [Энди Гутманс](https://ru.wikipedia.org/wiki/%D0%93%D1%83%D1%82%D0%BC%D0%B0%D0%BD%D1%81,_%D0%AD%D0%BD%D0%B4%D0%B8) и [Зеев Сураски](https://ru.wikipedia.org/wiki/%D0%A1%D1%83%D1%80%D0%B0%D1%81%D0%BA%D0%B8,_%D0%97%D0%B5%D0%B5%D0%B2), полностью переписали код интерпретатора. Новая версия языка PHP 3.0 была выпущена в июне 1998 года. С тех пор язык привлёк много сторонников, внёсшие вклад над разработкой новых модулей и API. Сегодня язык разрабатывается группой энтузиастов и распространяется под собственной лицензией в рамках проекта с открытым исходным кодом.

## Интерпретатор PHP

Местоположение интерпретатора можно узнать по информации выдаваемой функцией `phpinfo()`. В системе Windows путь к директории, где установлен PHP можно узнать в `Переменные среды` системы, путь к интерпретатору будет в списке переменной среды `Path`.

Стандартный при поставке PHP содержит больше 9000 функций. При разработке нет необходимости в таком количестве функций, поэтому часть стандартных функций вшиты в ядро, а часть функций (расширений) находятся в папке `ext`. В зависимости от необходимости мы можем подключать то или иное расширение.

`php.ini` – главный конфигурационный файл настроек PHP. При установке нужно при необходимости переименовать файл `php.ini-production` или `php.ini-development` в `php.ini`.

Настройки PHP делятся на три группы где их можно изменять:
1. Это непосредственно файл `php.ini`. На shared-хостинге, как правило, не дают прямой доступ к файлу `php.ini`, лишь выносят часть их настроек в панель управления хостингом;
2. `.htaccess` – настройки распространяются ко всем подпапкам, кроме тех, у которых есть свой файл `.htaccess`;
3. Прямо в коде. Например:

```php
<?php php_flag short_open_tag off ?>
```

### Связка с веб–сервером Apache

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

// Короткая запись. Сокращенное echo. Для этого должен быть подключен short_open_tag
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

// Выведет переменную beer, т.к. апостроф не допустимый символ в именах переменных
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

PHP поддерживает восемь простых типов:

* Четыре скалярных типа:
  * boolean (логический)
  * integer (целые числа)
  * float (вещественные (плавающие) числа)
  * string (строковый тип)

* Два смешанных типа:
  * array (массив)
  * object (объект)

* Два специальных типа:
  * resource
  * NULL
  * callable

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

Это очень похоже на использование HTML-тэга <PRE>,
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
$last = strlen($str); // Возвращает количество символов (отсчет с единицы)
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

Операторы бывают _унарные_ – принимают только одно значение, например `!` (логическое отрицание), `++` (инкремент) и др. и _бинарные_ – принимают два значения (большинство операторов именно такие). Также существует один _тернарный оператор_, принимающий три значения.

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
$a === $b // -> true если $a равно $b с учетом типа
$a != $b // -> true если $a не равно $b
$a !== $b // -> true если $a не равно $b или в случае, если они разных типов
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