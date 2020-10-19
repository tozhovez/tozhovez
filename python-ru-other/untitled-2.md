# Что такое Питонский способ введения зависимостей? - python

## Что такое Питонский способ введения зависимостей?

## Вступление

Для Java инъекция зависимостей работает как чистый OOP, т. е. вы предоставляете интерфейс для реализации и в своем коде платформы принимаете экземпляр класса, который реализует определенный интерфейс.

Теперь для Python вы можете сделать то же самое, но я думаю, что этот метод был слишком накладным в случае Python. Так как же тогда вы будете реализовывать его на Питонском языке?

## вариант использования

Скажем, это рамочный код:

```text
class FrameworkClass():
    def __init__(self, ...):
        ...

    def do_the_job(self, ...):
        # some stuff
        # depending on some external function
```

## базовый подход

Самый наивный \(а может быть, и самый лучший?\) способ состоит в том, чтобы потребовать, чтобы внешняя функция была передана в конструктор `FrameworkClass` , а затем вызвана из метода `do_the_job` .

 _**Рамочный Кодекс**_ :

```text
class FrameworkClass():
    def __init__(self, func):
        self.func = func

    def do_the_job(self, ...):
        # some stuff
        self.func(...)
```

 _**клиентский код**_ :

```text
def my_func():
    # my implementation

framework_instance = FrameworkClass(my_func)
framework_instance.do_the_job(...)
```

## Вопрос

Вопрос короткий. Есть ли какой-нибудь более широко используемый Питонский способ сделать это? Или, может быть, какие-то библиотеки, поддерживающие такую функциональность?

## UPDATE: Конкретная Ситуация

Представьте, что я разрабатываю микро - веб-фреймворк, который обрабатывает аутентификацию с помощью токенов. Эта структура нуждается в функции для предоставления некоторого `ID` , полученного из токена, и получения пользователя, соответствующего этому `ID` .

Очевидно, что фреймворк ничего не знает о пользователях или любой другой логике конкретного приложения, поэтому клиентский код должен ввести функциональность user getter в фреймворк, чтобы заставить аутентификацию работать.

[python](https://coderoad.ru/list/?page=1&sort=view&tag=python) [oop](https://coderoad.ru/list/?page=1&sort=view&tag=oop) [authentication](https://coderoad.ru/list/?page=1&sort=view&tag=authentication) [dependency-injection](https://coderoad.ru/list/?page=1&sort=view&tag=dependency-injection) [frameworks](https://coderoad.ru/list/?page=1&sort=view&tag=frameworks)

[ Источник](https://stackoverflow.com/questions/31678827) **bagrat**     28 июля 2015 в 14:10

###  5 Ответов

  
 44  
  


Смотрите на [Раймонда Хеттингера-супер-супер! - PyCon 2015](https://www.youtube.com/watch?v=EiOglTERPEo) для аргумента о том, как использовать супер и множественное наследование вместо DI. Если у вас нет времени, чтобы посмотреть все видео целиком, перейдите к минуте 15 \(но я бы рекомендовал посмотреть все это\).

Вот пример того, как применить то, что описано в этом видео, к вашему примеру:

 _**Рамочный Кодекс**_ :

```text
class TokenInterface():
    def getUserFromToken(self, token):
        raise NotImplementedError

class FrameworkClass(TokenInterface):
    def do_the_job(self, ...):
        # some stuff
        self.user = super().getUserFromToken(...)
```

 _**клиентский код**_ :

```text
class SQLUserFromToken(TokenInterface):
    def getUserFromToken(self, token):      
        # load the user from the database
        return user

class ClientFrameworkClass(FrameworkClass, SQLUserFromToken):
    pass

framework_instance = ClientFrameworkClass()
framework_instance.do_the_job(...)
```

Это будет работать, потому что Python MRO будет гарантировать, что вызывается метод клиента getUserFromToken \(если используется super\(\)\). Код придется изменить, если вы находитесь на Python 2.x.

Одно дополнительное преимущество здесь заключается в том, что это вызовет исключение, если клиент не предоставляет реализацию.

Конечно, это не совсем инъекция зависимостей, это множественное наследование и миксины, но это Питонский способ решить вашу проблему.

 **Serban Teodorescu**     09 августа 2015 в 21:38

  
 15  
  


Способ, которым мы делаем инъекцию зависимостей в нашем проекте, заключается в использовании [inject](https://pypi.python.org/pypi/Inject/3.1.1) lib. Ознакомьтесь с [документацией](https://pypi.python.org/pypi/Inject/3.1.1) . Я очень рекомендую использовать его для DI. Это вроде бы не имеет смысла только с одной функцией, но начинает иметь много смысла, когда вам приходится управлять несколькими источниками данных и т. д.

Следуя вашему примеру, это может быть что-то похожее на:

```text
# framework.py
class FrameworkClass():
    def __init__(self, func):
        self.func = func

    def do_the_job(self):
        # some stuff
        self.func()
```

Ваша пользовательская функция:

```text
# my_stuff.py
def my_func():
    print('aww yiss')
```

Где-то в приложении вы хотите создать загрузочный файл, который отслеживает все определенные зависимости:

```text
# bootstrap.py
import inject
from .my_stuff import my_func

def configure_injection(binder):
    binder.bind(FrameworkClass, FrameworkClass(my_func))

inject.configure(configure_injection)
```

И тогда вы могли бы использовать код таким образом:

```text
# some_module.py (has to be loaded with bootstrap.py already loaded somewhere in your app)
import inject
from .framework import FrameworkClass

framework_instance = inject.instance(FrameworkClass)
framework_instance.do_the_job()
```

Я боюсь, что это настолько питонно, насколько это может быть \(модуль имеет некоторую сладость python, как декораторы, чтобы ввести параметр и т. д.-проверьте документы\), так как python не имеет причудливых вещей, таких как интерфейсы или намеки типа.

Поэтому **ответить прямо на ваш вопрос** будет очень трудно. Я думаю, что истинный вопрос заключается в следующем: есть ли у python какая-то собственная поддержка для DI? И ответ, К сожалению, таков: нет.

 **Piotr Mazurek**     04 августа 2015 в 15:34

  
 6  
  


Некоторое время назад я написал микрограмму инъекции зависимостей с намерением сделать ее Pythonic - [Dependency Injector](https://github.com/ets-labs/python-dependency-injector). Именно так может выглядеть ваш код в случае его использования:

```text
"""Example of dependency injection in Python."""

import logging
import sqlite3

import boto.s3.connection

import example.main
import example.services

import dependency_injector.containers as containers
import dependency_injector.providers as providers


class Platform(containers.DeclarativeContainer):
    """IoC container of platform service providers."""

    logger = providers.Singleton(logging.Logger, name='example')

    database = providers.Singleton(sqlite3.connect, ':memory:')

    s3 = providers.Singleton(boto.s3.connection.S3Connection,
                             aws_access_key_id='KEY',
                             aws_secret_access_key='SECRET')


class Services(containers.DeclarativeContainer):
    """IoC container of business service providers."""

    users = providers.Factory(example.services.UsersService,
                              logger=Platform.logger,
                              db=Platform.database)

    auth = providers.Factory(example.services.AuthService,
                             logger=Platform.logger,
                             db=Platform.database,
                             token_ttl=3600)

    photos = providers.Factory(example.services.PhotosService,
                               logger=Platform.logger,
                               db=Platform.database,
                               s3=Platform.s3)


class Application(containers.DeclarativeContainer):
    """IoC container of application component providers."""

    main = providers.Callable(example.main.main,
                              users_service=Services.users,
                              auth_service=Services.auth,
                              photos_service=Services.photos)
```

Вот ссылка на более подробное описание этого примера- [http://python-dependency-injector.ets-labs.org/examples/services\_miniapp.html](http://python-dependency-injector.ets-labs.org/examples/services_miniapp.html)

Надеюсь, это немного поможет. Для получения дополнительной информации, пожалуйста, посетите:

* GitHub [https://github.com/ets-labs/python-dependency-injector](https://github.com/ets-labs/python-dependency-injector)
* Документы [http://python-dependency-injector.ets-labs.org](http://python-dependency-injector.ets-labs.org/)/

 **Roman Mogylatov**     26 февраля 2016 в 11:35

  
 1  
  


Я думаю, что DI и, возможно, AOP обычно не считаются Питоническими из-за типичных предпочтений разработчиков Python, а скорее из-за особенностей языка.

На самом деле вы можете реализовать [базовый фреймворк DI в линиях &lt;100](https://github.com/soulrebel/diy/blob/master/diy.py) , используя метаклассы и декораторы классов.

Для менее инвазивного решения эти конструкции можно использовать для включения пользовательских реализаций в общую структуру.

 **Andrea Ratto**     10 августа 2015 в 20:07

  
 0  
  


Из-за реализации Python OOP IoC и внедрения зависимостей не являются распространенными практиками в мире Python. Тем не менее этот подход казался многообещающим даже для Python.

* Использование зависимостей в качестве аргументов даже в том случае, если это класс, определенный в одной и той же кодовой базе, является резко непифоническим подходом. Python - это язык OOP с красивой и элегантной моделью OOP, поэтому игнорировать его не очень хорошая идея.
* Определять классы, полные абстрактных методов, просто имитируя тип интерфейса, тоже странно.
* Огромные обходные пути wrapper-on-wrapper слишком менее элегантны, чтобы их использовать.
* Я также не люблю использовать библиотеки, когда все, что мне нужно, - это небольшой шаблон.

Так что мое [решение](https://gist.github.com/I159/59c9b5358825bb3fdd0d2bec5db3a814) таково:

```text
# Framework internal
def MetaIoC(name, bases, namespace):
    cls = type("IoC{}".format(name), tuple(), namespace)
    return type(name, bases + (cls,), {})


# Entities level                                        
class Entity:
    def _lower_level_meth(self):
        raise NotImplementedError

    @property
    def entity_prop(self):
        return super(Entity, self)._lower_level_meth()


# Adapters level
class ImplementedEntity(Entity, metaclass=MetaIoC):          
    __private = 'private attribute value'                    

    def __init__(self, pub_attr):                            
        self.pub_attr = pub_attr                             

    def _lower_level_meth(self):                             
        print('{}\n{}'.format(self.pub_attr, self.__private))


# Infrastructure level                                       
if __name__ == '__main__':                                   
    ENTITY = ImplementedEntity('public attribute value')     
    ENTITY.entity_prop         
```

 **I159**     25 июля 2018 в 16:35

[Правильный способ введения зависимостей?](https://coderoad.ru/9450931/Правильный-способ-введения-зависимостей)

 Какой из них является лучшим способом инъекции моих зависимостей? Почему? В чем разница между ними? public abstract class Service { private IConfig config; @Inject public Service\(IConfog config\) {...

[Что такое свойство зависимостей?](https://coderoad.ru/617312/Что-такое-свойство-зависимостей)

 Что такое свойство зависимостей в .Net \(особенно в контексте WPF\). В чем отличие от обычного объекта недвижимости?

[Использование Guice для введения зависимостей в конструктор действия Android](https://coderoad.ru/2525969/Использование-Guice-для-введения-зависимостей-в-конструктор-действия-Android)

 Кто-нибудь знает способ использования Guice для введения зависимостей в конструктор действия в Android? Похоже, что действия обычно имеют только конструктор по умолчанию, так что платформа может...

[Перепроектирование зависимостей только что десериализованного объекта](https://coderoad.ru/2675579/Перепроектирование-зависимостей-только-что-десериализованного-объекта)

 Если программа буквально только что десериализовала объект \(не имеет значения, как, но просто скажите BinaryFormatter был использован\). Что такое хороший дизайн для повторного введения зависимостей...

[Питонский способ делать это?](https://coderoad.ru/17215055/Питонский-способ-делать-это)

 У меня есть такая конструкция кода: flag = True while flag: do\_something\(\) if some\_condition: flag = False Разве это лучший способ сделать это? Или есть лучший питонский способ?

[Что такое свойство зависимостей? В чем его польза?](https://coderoad.ru/5390331/Что-такое-свойство-зависимостей-В-чем-его-польза)

 Возможность : Что такое свойство зависимостей? Что такое свойство зависимостей? Чем он отличается от обычного свойства? Какова цель свойств зависимостей? И почему он используется, когда он...

[Питонский способ записи, если открытие успешно](https://coderoad.ru/35975921/Питонский-способ-записи-если-открытие-успешно)

 Я ищу питонский способ создания и записи в файл, если его открытие было успешным, или же возвращаю ошибку, если нет \(например, отказано в разрешении\). Я читал здесь What's питонский способ...

[Питонский способ добавления столбцов в массив](https://coderoad.ru/33402331/Питонский-способ-добавления-столбцов-в-массив)

 Я пытаюсь объединить массив m x n , называемый data , со списком m элементов, называемых cluster\_data , так что каждый элемент в списке cluster\_data добавляется как последний элемент каждой из строк...

[Лучший питонский способ очистить строку](https://coderoad.ru/35796708/Лучший-питонский-способ-очистить-строку)

 У меня есть такая веревочка: inconnue \(1h 30min\) Я ищу лучший питонский способ \(элегантный способ\) извлечь эти 2 строки inconnue и 1h 30min из строки, полной пробелов и ломаных линий

[Что такое питонский способ записи строк в файл?](https://coderoad.ru/20868484/Что-такое-питонский-способ-записи-строк-в-файл)

 В чем разница между использованием File.write\(\) и print&gt;&gt;File, ? Какой питонский способ записи в файл? &gt;&gt;&gt; with open\('out.txt','w'\) as fout: ... fout.write\('foo bar'\) ... &gt;&gt;&gt;...

