# Python Language - os.path \| python Tutorial

## Вступление <a id="----------"></a>

 Этот модуль реализует некоторые полезные функции для путей. Параметры пути могут передаваться как строки, так и байты. Приложениям рекомендуется представлять имена файлов в виде \(Unicode\) символьных строк.

## Синтаксис <a id="---------"></a>

*  os.path.join \(a, \* p\)
*  os.path.basename \(р\)
*  os.path.dirname \(р\)
*  os.path.split \(р\)
*  os.path.splitext \(р\)

## Присоединительные пути <a id="----------------------"></a>

 Чтобы объединить два или более компонентов пути вместе, сначала импортируйте os-модуль python, а затем используйте следующее:

```text
import os
os.path.join('a', 'b', 'c')
```

 Преимущество использования os.path заключается в том, что он позволяет коду оставаться совместимым во всех операционных системах, поскольку в нем используется разделитель, подходящий для платформы, на которой он работает.

 Например, результатом этой команды в Windows будет:

```text
>>> os.path.join('a', 'b', 'c')
'a\b\c'
```

 В ОС Unix:

```text
>>> os.path.join('a', 'b', 'c')
'a/b/c'
```

## Абсолютный путь от относительного пути <a id="--------------------------------------"></a>

 Использовать `os.path.abspath` :

```text
>>> os.getcwd()
'/Users/csaftoiu/tmp'
>>> os.path.abspath('foo')
'/Users/csaftoiu/tmp/foo'
>>> os.path.abspath('../foo')
'/Users/csaftoiu/foo'
>>> os.path.abspath('/foo')
'/foo'
```

## Манипуляция компонентов пути <a id="----------------------------"></a>

 Чтобы разбить один компонент на путь:

```text
>>> p = os.path.join(os.getcwd(), 'foo.txt')
>>> p
'/Users/csaftoiu/tmp/foo.txt'
>>> os.path.dirname(p)
'/Users/csaftoiu/tmp'
>>> os.path.basename(p)
'foo.txt'
>>> os.path.split(os.getcwd())
('/Users/csaftoiu/tmp', 'foo.txt')
>>> os.path.splitext(os.path.basename(p))
('foo', '.txt')
```

## Получить родительский каталог <a id="-----------------------------"></a>

```text
os.path.abspath(os.path.join(PATH_TO_GET_THE_PARENT, os.pardir))
```

## Если данный путь существует. <a id="----------------------------"></a>

 проверить, существует ли данный путь

```text
path = '/home/john/temp'
os.path.exists(path)
#this returns false if path doesn't exist or if the path is a broken symbolic link
```

## проверьте, является ли данный путь каталогом, файлом, символической ссылкой, точкой монтирования и т. д. <a id="--------------------------------------------------------------------------------------------------------"></a>

 проверить, является ли данный путь каталогом

```text
dirname = '/home/john/python'
os.path.isdir(dirname)
```

 чтобы проверить, является ли данный путь файлом

```text
filename = dirname + 'main.py'
os.path.isfile(filename)
```

 чтобы проверить, является ли данный путь [символической ссылкой](https://en.wikipedia.org/wiki/Symbolic_link)

```text
symlink = dirname + 'some_sym_link'
os.path.islink(symlink)
```

 чтобы проверить, является ли данный путь [точкой монтирования](http://www.linuxtopia.org/online_books/introduction_to_linux/linux_Mount_points.html)

```text
mount_path = '/home'
os.path.ismount(mount_path)
```

