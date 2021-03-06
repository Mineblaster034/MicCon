﻿# PARADIGM

- список команд оболочки привден в файле shell.md
- оболочка может интерпретировать скрипты (подробнее в файле shell.md)
- оболочка может исполнять программы, написанные и скомпилированые для MocConOS
- исходный код программы пишется на assembly-like языке и транслируется (компилируется ассемблером, макропроцессором) в объектный файл, содержащий интерпретируемый байт-код
- среда исполнения (интерпретации) байт-кода представляет собой регистровую виртуальную машину (подобную Java, Dalvik, .Net)
- при выполнении программы используется массив переменных (регистры по своей сути) и стек вызовов
- размер стека вызовов - 254, количество вложенных подпрограмм во время выполнения не должно превышать это количество
- имена переменных и меток должны начинаться с маленькой или большой латинской буквы и состоять из букв, цифр и символа "_"
- все переменные глобальны, всего uint8_max = 256 переменных (регистров)
- все переменные имеют тип int16, объявляются как массив размерности uint8 > 0 с мусорными начальными значениями
- индексация массивов с 0
- обращение к массиву без указания номера элемента эквивалентна указанию нулевого уэлемента
- однострочный комментарий после ';'
- длинна "машинного слова" 8 бит
- первый бит в команде - тип аргумента, остальные 7 - опкод операции
- всего возможно 2^7 = 128 команды (RISC-принцип), из них используется 57, свободно 71
- типы аргументов в байт-коде: 
	- 0 - uint16 / метка / null (2 или 0 байт)
	- 1 - переменная (1 байт) 
- int16 передаются и хранятся в байт-коде с порядком big-endian
- при выводе символов используется ASCII кодировка, учитывается только младший байт int16-числа
- файловая система - fat16 (или fat32, с теми же ограничениями, что и fat16)
- имена файлов записываются массивом (с обязательным нуль терминатором) в нотации 8.3, каталоги и файлы разграничиваются символом '/'
- рабочий каталог - всегда корневой каталог SD-карты, его можно не указывать при обращении к файлу
- в качестве символа переноса строки используется '\n', все строки обязательно оканчиваются нуль терминатором
- подпрограмма начинается с (prс) и заканчивается (ret)
- условный (if*) и безусловный (jmp) переход по метке отличается от вызова подпрограммы (prc) тем, что не вносит текущий адрес в стек вызовов
- отсутствует проверка подпрограмма-метка, т.е. "безусловный переход по подпрограмме" и "вызов метки" не вызовут никаких ошибок
- интерпретатор использует 16-bit адресацию, размер итогового объектного файла не должен превышать 2^16=65536 байт (исполняемые файлы могут быть больше этого размера, однако переход по меткам и вызов подпрограмм из/в сверх этого размера будет невозможен)
- команды в исходном коде записываются в виде 
	- OPERATION {ARG_1{\[X]} {ARG_2{[Y]}}} {;COMMENT}
	- OPERATION - имя команды
	- ARG_1 - первый аргумент команды
	- ARG_2 - второй аргумент операции
	- X, Y - индекс элемента при работе с массивами
	- COMMENT - однострочный комментарий, необязателен
- список свободных пинов
	- Digital: 0 1 14 15 16 17 18 19 42 43 44 45 46 47 48 49 53
	- PWM: 8 9 10 11 12 13
	- Analog in: 54 55 56 57 58 59 60 61 62 63 64 65
	- I2C: 20 21
	- Other: 66 67 68 69
	- Due only: 50 51 52
