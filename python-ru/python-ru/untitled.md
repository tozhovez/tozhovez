# 2. Использование интерпретатора Python

## 2.1. Запуск интерпретатора[¶]()

Интерпретатор Python после установки располагается, обычно, по пути `/usr/local/bin/python3.8` — на тех компьютерах, где этот путь доступен. Добавление каталога `/usr/local/bin` к пути поиска Unix-шелла позволит запустить интерпретатор набором команды

прямо из шелла. [\[1\]]() Поскольку выбор каталога, в котором будет обитать интерпретатор, осуществляется при его установке, то возможны и другие варианты — посоветуйтесь с вашим Python-гуру или системным администратором. \(Например, путь `/usr/local/python` тоже популярен в качестве альтернативного расположения.\)

На компьютерах с Windows, на которых установлен Python из [Microsoft Store](https://digitology.tech/docs/python_3/using/windows.html#windows-store) будет доступна команда: `python3.8`. Если у вас установлен [py.exe запускальщик](https://digitology.tech/docs/python_3/using/windows.html#launcher), вы можете использовать `py` команду. Другие способы запуска [Exursus: задание переменных среды](https://digitology.tech/docs/python_3/using/windows.html#setting-envvars) см. в разделе Python.

При наборе символа конца файла \(Control-D в Unix, Control-Z Windows\) в ответ на основное приглашение интерпретатора, последний будет вынужден закончить работу с нулевым статусом выхода. Если это не сработает — вы можете выйти из интерпретатора путём ввода следующей команды: `quit()`.

Функции редактирования строк интерпретатора включают интерактивное редактирование, замену истории и завершение кода в системах, поддерживающих библиотеку [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html).Самый быстрый, наверное, способ проверить, поддерживается ли расширенное редактирование командной строки, заключается в нажатии Control-P в ответ на первое полученное приглашение Python. Если вы услышите звуковой сигнал, значит вам доступно редактирование командной строки; Введение в ключи см. в Приложении [Интерактивное редактирование ввода и подстановка из истории](https://digitology.tech/docs/python_3/tutorial/interactive.html#tut-interacting). Если на ваш взгляд ничего не произошло или отобразился символ `^P` — редактирование командной строки недоступно — удалять символы из текущей строки возможно будет лишь использованием клавиши Backspace.

Интерпретатор ведёт себя сходно шеллу Unix: если он вызван, когда стандартный ввод привязан к устройству tty — он считывает и выполняет команды в режиме диалога; будучи вызванным с именем файла в качестве параметра или с файлом, назначенным на стандартный ввод — он читает и выполняет _сценарий_ из этого файла.

Другой способ запустить интерпретатор — `python -c command [arg] ...`, — при её использовании поочередно выполняются инструкции\(-ция\) из _command_ \(как при использовании опции [`-c`](https://digitology.tech/docs/python_3/using/cmdline.html#cmdoption-c) Unix-шелла\). В связи с тем, что инструкции Python часто содержат пробелы или другие специальные для шелла символы, рекомендуется полностью заключать _command_ в одинарные кавычки.

Некоторые модули Python оказываются полезными при использовании их в качестве сценариев. Они могут быть запущены в виде командой `python -m module [arg] ...`, — таким образом исполняется исходный файл модуля _module_ \(как произошло бы, если бы вы ввели его полное имя в командной строке\).

При использовании файла сценария иногда полезно иметь возможность запустить сценарий и затем войти в интерактивный режим. Это может быть сделано через указание параметра [`-i`](https://digitology.tech/docs/python_3/using/cmdline.html#cmdoption-i) перед именем сценария.

Все опции командной строки описаны в [Командная строка и среда](https://digitology.tech/docs/python_3/using/cmdline.html#using-on-general).

### 2.1.1. Передача параметров[¶]()

В случае, если интерпретатору известны имя сценария и дополнительные параметры, с которыми он вызван, все они передаются сценарию в переменной `argv` модуля `sys`, представляющей собой список строк. Вы можете получить доступ к этому списку, выполнив `import sys`. Длина списка — минимум, единица; если не переданы ни имя сценария, ни аргументы — то `sys.argv[0]` содержит пустую строку. Когда в качестве имени сценария передан `'-'` \(означает стандартный ввод\), `sys.argv[0]` устанавливается в `'-'`. Если используется директива [`-c`](https://digitology.tech/docs/python_3/using/cmdline.html#cmdoption-c) _command_, то `sys.argv[0]` устанавливается как `'-c'`. Когда используется [`-m`](https://digitology.tech/docs/python_3/using/cmdline.html#cmdoption-m) _module_ , то `sys.argv[0]` устанавливается равным полному имени модуля по расположению. Опции, обнаруженные после сочетаний [`-c`](https://digitology.tech/docs/python_3/using/cmdline.html#cmdoption-c) _command_ или [`-m`](https://digitology.tech/docs/python_3/using/cmdline.html#cmdoption-m) _module_ не обрабатываются интерпретатором Python, но остаются в переменной `sys.argv`, чтобы обеспечить возможность отслеживания в самой команде или в модуле.

### 2.1.2. Интерактивный режим[¶]()

Когда команды считываются из tty, интерпретатор находится в _интерактивном режиме_. В этом режиме запрашивается следующая команда с _основного приглашения_, обычно три знака больше \(`>>>`\); в то же время, для продолжающих строк выводится _вспомогательное приглашение_, по умолчанию три точки \(`...`\). Перед выводом первого приглашения интерпретатор отображает приветственное сообщение, содержащее номер его версии и пометку о правах копирования:

```text
$ python3.8
Python 3.8 (default, Sep 16 2015, 09:25:04)
[GCC 4.8.2] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

_Продолжающие строки_ используются в случаях, когда необходимо ввести многострочную конструкцию. Взгляните, например, на следующий оператор [`if`](https://digitology.tech/docs/python_3/reference/compound_stmts.html#if):

```text
>>> the_world_is_flat = True
>>> if the_world_is_flat:
...     print("Be careful not to fall off!")
...
Be careful not to fall off!
```

Подробнее об интерактивном режиме смотрите [Интерактивный режим](https://digitology.tech/docs/python_3/tutorial/appendix.html#tut-interac).

## 2.2. Интерпретатор и его окружение[¶]()

### 2.2.1. Кодировка исходных файлов[¶]()

По умолчанию, исходники Python считаются созданными в кодировке UTF-8. В этой кодировке в строковых литералах, идентификаторах и комментариях могут быть использованы символы большинства языков мира — хотя стандартная библиотека Python использует только символы ASCII для именования идентификаторов — и этому соглашению должен следовать любой переносимый код. Для корректного отображения всех этих символов, ваш редактор должен опознавать файл как закодированный в UTF-8 и должен использовать шрифт, который содержит все символы, используемые в файле.

Для объявления кодировки, отличной от кодировки по умолчанию, используется специальная строка комментария которую следует добавить в качестве _первой_ строки файла. Синтаксис выглядит следующим образом:

> \# -_- coding: encoding -_-

Где _encoding_ является одной из допустимых в [`codecs`](https://digitology.tech/docs/python_3/library/codecs.html#module-codecs), поддерживаемых Python.

Например, если ваш текстовый редактор не поддерживает кодировку файлов UTF-8 и настаивает на какой-либо другой кодировке, скажем, Windows-1252, можно написать:

> \# -_- coding: cp1252 -_-

Исключение из правила если _первая строка_ в исходном коде начинается с [UNIX «shebang» строки](https://digitology.tech/docs/python_3/tutorial/appendix.html#tut-scripts). В этом случае объявление кодировки должно быть добавлено в качестве второй строки файла. Например:

```text
#!/usr/bin/env python3
# -*- coding: cp1252 -*-
```

Сноски

