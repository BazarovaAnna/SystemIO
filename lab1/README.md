# Лабораторная работа 1

**Название:** "Разработка драйверов символьных устройств"

**Цель работы:** получить знания и навыки разработки драйверов символьных устройств для операционной системы Linux

## Описание функциональности драйвера

* Драйвер должен создавать символьное устройство /dev/var4

* Драйвер должен создавать интерфейс для получения сведений о результатах операций над созданным символьным устройством: файл /proc/var4.

* Должен обрабатывать операции записи и чтения в соответствии с вариантом задания (варианты представлены ниже).

* При записи текста в файл символьного устройства должно запоминаться количество пробелов во введенном тексте. Последовательность полученных результатов с момента загрузки модуля ядра должна выводиться при чтении созданного файла /proc/varN в консоль пользователя.

* При чтении из файла символьного устройства в кольцевой буфер ядра должен осуществляться вывод тех же данных, которые выводятся при чтении файла /proc/varN.


## Инструкция по сборке

1. Скачать репозиторий https://github.com/BazarovaAnna/SystemIO

2. cd lab1

3. make

## Инструкция пользователя

**Иногда, если все не работает имеет смысл перезагрузить модуль!**

### Инициализировать драйвер следующей командой:

sudo insmod lab1.ko

### Проверить сообщения:

dmesg

Должно появиться "\[VAR4\]:initialized"

### Проверяем права символьного устройства

ls -la /dev/var4

### Работа с символьным устройством:

echo "\<Message\>" > /dev/var4 && cat/proc/var4

**Важно!** Буфер ограничен по размерам, \<Message\> не должно быть длиннее 100 символов

### Деинициализировать драйвер:

sudo rmmod lab1

(dmesg должен показать "\[VAR4\]:exit")

## Примеры использования

mint@mint:\~/Desktop/sysio$ cd lab1

mint@mint:\~/Desktop/sysio/lab1$ make

mint@mint:\~/Desktop/sysio/lab1$ ls

Makefile        README.md  lab1.ko     lab1.mod.o  modules.order

Module.symvers  lab1.c     lab1.mod.c  lab1.o

mint@mint:\~/Desktop/sysio/lab1$ sudo insmod lab1.ko

mint@mint:\~/Desktop/sysio/lab1$ dmesg

\<...\>

\[ 3157.090053\] \[VAR4\]: initialized

mint@mint:\~/Desktop/sysio/lab1$ ls -la /dev/var4

crw-rw-rw- 1 root root 243, 0 Feb 26 09:29 /dev/var4

mint@mint:\~/Desktop/sysio/lab1$ echo "h i hel lo" > /dev/var4 && cat /proc/var4

\[VAR4\]: 3 

mint@mint:\~/Desktop/sysio/lab1$ sudo rmmod lab1

mint@mint:\~/Desktop/sysio/lab1$ dmesg

\<...\>

\[ 3615.716860\] \[VAR4\]: exit
