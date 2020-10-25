# Python Language - Метаклассы \| python Tutorial

### Вступление <a id="----------"></a>

 Метаклассы позволяют вам глубоко модифицировать поведение классов Python \(с точки зрения их определения, создания экземпляров, доступа и т. Д.\) Путем замены метакласса `type` который новые классы используют по умолчанию.

### замечания <a id="---------"></a>

 При проектировании вашей архитектуры учитывайте, что многие вещи, которые могут выполняться с помощью метаклассов, также могут быть выполнены с использованием более простой семантики:

*  Традиционного наследования часто более чем достаточно.
*  Декораторы классов могут сочетать функциональность с классами по специальному подходу.
*  Python 3.6 представляет `__init_subclass__()` который позволяет классу участвовать в создании своего подкласса.

### Основные метаклассы <a id="-------------------"></a>

 Когда `type` вызывается с тремя аргументами, он ведет себя как класс \(meta\) и создает новый экземпляр, т. Е. он создает новый класс / тип.

```text
Dummy = type('OtherDummy', (), dict(x=1))
Dummy.__class__              #  
Dummy().__class__.__class__  #  
```

 `type` подкласса можно создать для пользовательского метакласса.

```text
class mytype(type):
    def __init__(cls, name, bases, dict):
        # call the base initializer
        type.__init__(cls, name, bases, dict)

        # perform custom initialization...
        cls.__custom_attribute__ = 2
```

 Теперь у нас есть новый `mytype` который можно использовать для создания классов таким же образом, как и `type` .

```text
MyDummy = mytype('MyDummy', (), dict(x=2))
MyDummy.__class__              # 
MyDummy().__class__.__class__  # 
MyDummy.__custom_attribute__   # 2
```

 Когда мы создаем новый класс с использованием ключевого слова `class` метаклас по умолчанию выбирается на основе базовых классов.

```text
>>> class Foo(object):
...     pass

>>> type(Foo)
type
```

 В приведенном выше примере единственным базовым классом является `object` поэтому наш метакласс будет типом `object` , который является `type` . Можно переопределить значение по умолчанию, однако это зависит от того, используем ли мы Python 2 или Python 3:

Python 2.x 2.7

 Для определения метакласса можно использовать специальный атрибут `__metaclass__` .

```text
class MyDummy(object):
    __metaclass__ = mytype
type(MyDummy)  # 
```

Python 3.x 3.0

 Специальный `metaclass` аргумент ключевого слова определяет метакласс.

```text
class MyDummy(metaclass=mytype):
    pass
type(MyDummy)  # 
```

 Любые аргументы ключевых слов \(кроме `metaclass` \) в объявлении класса будут переданы в метакласс. Таким образом, `class MyDummy(metaclass=mytype, x=2)` передаст `x=2` в качестве аргумента ключевого слова в конструктор `mytype` .

 Прочтите это [подробное описание метаклассов python](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python/6581949#6581949) для более подробной информации.

### Синглтоны, использующие метаклассы <a id="----------------------------------"></a>

 Синглтон - это шаблон, который ограничивает экземпляр класса одним экземпляром / объектом. Для получения дополнительной информации о шаблонах проектирования синглтона python см. [Здесь](http://python-3-patterns-idioms-test.readthedocs.io/en/latest/Singleton.html) .

```text
class SingletonType(type):
    def __call__(cls, *args, **kwargs):
        try:
            return cls.__instance
        except AttributeError:
            cls.__instance = super(SingletonType, cls).__call__(*args, **kwargs)
            return cls.__instance
```

Python 2.x 2.7

```text
class MySingleton(object):
    __metaclass__ = SingletonType
```

Python 3.x 3.0

```text
class MySingleton(metaclass=SingletonType):
    pass
```

```text
MySingleton() is MySingleton()  # True, only one instantiation occurs
```

### Использование метакласса <a id="------------------------"></a>

##  Синтаксис метакласса

Python 2.x 2.7

```text
class MyClass(object):
    __metaclass__ = SomeMetaclass
```

Python 3.x 3.0

```text
class MyClass(metaclass=SomeMetaclass):
    pass
```

##  Совместимость с Python 2 и 3 с `six`

```text
import six

class MyClass(six.with_metaclass(SomeMetaclass)):
    pass
```

### Пользовательские функции с метаклассами <a id="---------------------------------------"></a>

 **Функциональность в метаклассах** может быть изменена так, что всякий раз, когда создается класс, строка выводится на стандартный вывод или генерируется исключение. Этот метакласс отобразит имя создаваемого класса.

```text
class VerboseMetaclass(type):

    def __new__(cls, class_name, class_parents, class_dict):
        print("Creating class ", class_name)
        new_class = super().__new__(cls, class_name, class_parents, class_dict)
        return new_class
```

 Вы можете использовать метакласс следующим образом:

```text
class Spam(metaclass=VerboseMetaclass):
    def eggs(self):
        print("[insert example string here]")
s = Spam()
s.eggs()
```

 Стандартный выход будет:

```text
Creating class Spam
[insert example string here]
```

### Введение в метаклассы <a id="---------------------"></a>

###  Что такое метакласс?

 В Python все является объектом: целые числа, строки, списки, даже функции и классы сами по себе являются объектами. И каждый объект является экземпляром класса.

 Чтобы проверить класс объекта x, можно вызвать `type(x)` , поэтому:

```text
>>> type(5)

>>> type(str)

>>> type([1, 2, 3])


>>> class C(object):
...     pass
...
>>> type(C)
    
```

 Большинство классов в python являются экземплярами `type` . сам `type` также является классом. Такие классы, экземпляры которых также являются классами, называются метаклассами.

###  Самый простой метакласс

 ОК, так что уже один метаклассом в Python: `type` . Можем ли мы создать еще один?

```text
class SimplestMetaclass(type):
    pass

class MyClass(object):
    __metaclass__ = SimplestMetaclass
```

 Это не добавляет каких-либо функциональных возможностей, но это новый метакласс, см., Что MyClass теперь является экземпляром SimplestMetaclass:

```text
>>> type(MyClass)

```

###  Метакласс, который делает что-то

 Метакласс, который делает что-то обычно, переопределяет `type` `__new__` , чтобы изменить некоторые свойства создаваемого класса, прежде чем вызывать исходный `__new__` который создает класс:

```text
class AnotherMetaclass(type):
    def __new__(cls, name, parents, dct):
        # cls is this class
        # name is the name of the class to be created
        # parents is the list of the class's parent classes
        # dct is the list of class's attributes (methods, static variables)

        # here all of the attributes can be modified before creating the class, e.g.

        dct['x'] = 8  # now the class will have a static variable x = 8

        # return value is the new class. super will take care of that
        return super(AnotherMetaclass, cls).__new__(cls, name, parents, dct)
```

### Метакласс по умолчанию <a id="----------------------"></a>

 Возможно, вы слышали, что все в Python является объектом. Это правда, и все объекты имеют класс:

```text
>>> type(1)
int
```

 Литерал 1 является экземпляром `int` . Давайте объявим класс:

```text
>>> class Foo(object):
...    pass
...
```

 Теперь давайте создадим экземпляр:

```text
>>> bar = Foo()
```

 Что такое класс `bar` ?

```text
>>> type(bar)
Foo
```

 Ницца, `bar` - это пример `Foo` . Но каков класс самого `Foo` ?

```text
>>> type(Foo)
type
```

 Хорошо, сам `Foo` является экземпляром `type` . Как насчет `type` ?

```text
>>> type(type)
type
```

 Итак, что такое метакласс? Пока давайте притвориться, что это просто причудливое имя для класса класса. Takeaways:

*  Все это объект в Python, поэтому все имеет класс
*  Класс класса называется метаклассом
*  Метакласс по умолчанию - это `type` , и на сегодняшний день он является наиболее распространенным метаклассом

 Но почему вы должны знать о метаклассах? Ну, сам Python довольно «взломан», и концепция метакласса важна, если вы занимаетесь продвинутыми вещами, такими как мета-программирование, или если вы хотите контролировать, как инициализируются ваши классы.  
  


