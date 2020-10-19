# Что такое питоновский способ инъекции зависимостей? Ru Python

[Что такое питоновский способ инъекции зависимостей?]()

##  Введение

 Для Java Injection Dependency работает как чистый OOP, т. Е. Вы предоставляете интерфейс, который должен быть реализован, и в вашем коде кода принимайте экземпляр класса, который реализует определенный интерфейс.

 Теперь для Python вы можете сделать то же самое, но я думаю, что этот метод слишком высок в случае Python. Итак, как бы вы реализовали его на питоническом пути?

##  Случай использования

 Скажем, это код рамки:

```text
class FrameworkClass(): def __init__(self, ...): ... def do_the_job(self, ...): # some stuff # depending on some external function 
```

##  Основной подход

 Самый наивный \(и, может быть, лучший?\) Способ – потребовать, чтобы внешняя функция была `do_the_job` в конструктор `FrameworkClass` , а затем вызывается из метода `do_the_job` .

 _**Рамочный код:**_

```text
 class FrameworkClass(): def __init__(self, func): self.func = func def do_the_job(self, ...): # some stuff self.func(...) 
```

 _**Код клиента:**_

```text
 def my_func(): # my implementation framework_instance = FrameworkClass(my_func) framework_instance.do_the_job(...) 
```

##  Вопрос

 Вопрос короткий. Существует ли более широко используемый питонический способ? Или, может быть, библиотеки, поддерживающие такую ​​функциональность?

##  ОБНОВЛЕНИЕ: Конкретная ситуация

 Представьте себе, что я разрабатываю микро-веб-структуру, которая обрабатывает аутентификацию с помощью токенов. Эта структура нуждается в функции для доставки некоторого `ID` полученного из токена, и получения соответствующего ему `ID` .

 Очевидно, что фреймворк ничего не знает о пользователях или какой-либо другой специфической для приложения логике, поэтому клиентский код должен внедрить функциональность пользовательского геттера в фреймворк, чтобы сделать работу аутентификации.

* [Как я могу зарегистрировать пользователя при использовании repoze.who?](https://www.rupython.com/115759-115759.html)
* [Аутентификация hash\_hmac sha512 в python](https://www.rupython.com/hash_hmac-sha512-python-22308.html)
* [Как сказать браузеру забыть htdigest?](https://www.rupython.com/htdigest-49966.html)
* [Принятие адреса электронной почты в качестве имени пользователя в Django](https://www.rupython.com/3148-3148.html)
* [Как сделать файл приватным, закрепив URL-адрес, который могут видеть только прошедшие проверку подлинности пользователи](https://www.rupython.com/x435-68-13789.html)

 См. Раймонд Хеттингер – Супер считается супер! – PyCon 2015 для аргумента о том, как использовать супер и множественное наследование вместо DI. Если у вас нет времени на просмотр всего видео, перейдите к минуте 15 \(но я бы рекомендовал посмотреть все\).

 Вот пример того, как применить то, что описано в этом видео, к вашему примеру:

 _**Рамочный код:**_

```text
 class TokenInterface(): def getUserFromToken(self, token): raise NotImplementedError class FrameworkClass(TokenInterface): def do_the_job(self, ...): # some stuff self.user = super().getUserFromToken(...) 
```

 _**Код клиента:**_

```text
 class SQLUserFromToken(TokenInterface): def getUserFromToken(self, token): # load the user from the database return user class ClientFrameworkClass(FrameworkClass, SQLUserFromToken): pass framework_instance = ClientFrameworkClass() framework_instance.do_the_job(...) 
```

 Это будет работать, потому что MRO Python гарантирует, что вызывается метод client getUserFromToken \(если используется super \(\)\). Код должен измениться, если вы находитесь на Python 2.x.

 Одно из преимуществ заключается в том, что это приведет к возникновению исключения, если клиент не предоставит реализацию.

 Конечно, это не настоящая инъекция зависимостей, это множественное наследование и микшины, но это путинский способ решить вашу проблему. 

 То, как мы делаем инъекцию зависимости в нашем проекте, – это использование lib. Проверьте документацию . Я настоятельно рекомендую использовать его для DI. Это не имеет смысла только с одной функцией, но начинает иметь большой смысл, когда вам нужно управлять несколькими источниками данных и т. Д. И т. Д.

 После вашего примера это может быть нечто похожее на:

```text
 # framework.py class FrameworkClass(): def __init__(self, func): self.func = func def do_the_job(self): # some stuff self.func() 
```

 Ваша пользовательская функция:

```text
 # my_stuff.py def my_func(): print('aww yiss') 
```

 Где-то в приложении вы хотите создать файл начальной загрузки, который отслеживает все определенные зависимости:

```text
 # bootstrap.py import inject from .my_stuff import my_func def configure_injection(binder): binder.bind(FrameworkClass, FrameworkClass(my_func)) inject.configure(configure_injection) 
```

 И тогда вы можете использовать код таким образом:

```text
 # some_module.py (has to be loaded with bootstrap.py already loaded somewhere in your app) import inject from .framework import FrameworkClass framework_instance = inject.instance(FrameworkClass) framework_instance.do_the_job() 
```

 Я боюсь, что это так же возможно, как и pythonic \(модуль имеет некоторую сладость python, например, декораторы, чтобы вводить параметр и т. Д. – проверять документы\), поскольку у python нет модных вещей, таких как интерфейсы или намеки типа.

 Поэтому **ответить на ваш вопрос** напрямую было бы очень сложно. Я думаю, что истинный вопрос: у python есть некоторая встроенная поддержка DI? И ответ, к сожалению: нет. 

 Некоторое время назад я написал формула для инъекций зависимости с амбициями, чтобы сделать ее Pythonic – Injector Dependency Injector . Вот как ваш код может выглядеть в случае его использования:

```text
 """Example of dependency injection in Python.""" import logging import sqlite3 import boto.s3.connection import example.main import example.services import dependency_injector.containers as containers import dependency_injector.providers as providers class Platform(containers.DeclarativeContainer): """IoC container of platform service providers.""" logger = providers.Singleton(logging.Logger, name='example') database = providers.Singleton(sqlite3.connect, ':memory:') s3 = providers.Singleton(boto.s3.connection.S3Connection, aws_access_key_id='KEY', aws_secret_access_key='SECRET') class Services(containers.DeclarativeContainer): """IoC container of business service providers.""" users = providers.Factory(example.services.UsersService, logger=Platform.logger, db=Platform.database) auth = providers.Factory(example.services.AuthService, logger=Platform.logger, db=Platform.database, token_ttl=3600) photos = providers.Factory(example.services.PhotosService, logger=Platform.logger, db=Platform.database, s3=Platform.s3) class Application(containers.DeclarativeContainer): """IoC container of application component providers.""" main = providers.Callable(example.main.main, users_service=Services.users, auth_service=Services.auth, photos_service=Services.photos) 
```

 Вот ссылка на более подробное описание этого примера – [http://python-dependency-injector.ets-labs.org/examples/services\_miniapp.html](http://python-dependency-injector.ets-labs.org/examples/services_miniapp.html)

 Надеюсь, это поможет немного. Для получения дополнительной информации, пожалуйста, посетите:

*  GitHub [https://github.com/ets-labs/python-dependency-injector](https://github.com/ets-labs/python-dependency-injector)
*  Документы [http://python-dependency-injector.ets-labs.org/](http://python-dependency-injector.ets-labs.org/)

 Я думаю, что DI и, возможно, AOP обычно не считаются Pythonic из-за типичных предпочтений разработчиков Python, а скорее из-за особенностей языка.

 На самом деле вы можете реализовать базовую структуру DI в &lt;100 строк , используя метаклассы и декораторы классов.

 Для менее инвазивного решения эти конструкции могут быть использованы для подгонки пользовательских реализаций в общую структуру. 

* [Индивидуальная регистрация web2py](https://www.rupython.com/web2py-17-107388.html)
* [Аутентификация с помощью открытых ключей и cx\_Oracle с использованием Python](https://www.rupython.com/40537-40537.html)
* [Индивидуальная проверка подлинности Django Rest Framework](https://www.rupython.com/73551-73551.html)
* [Чтение https-url из Python с помощью аутентификации с базовым доступом](https://www.rupython.com/https-url-python-x4-99080.html)
* [Безопасная система аутентификации в python?](https://www.rupython.com/32302-32302.html)
* [nginx HTTP-аутентификация с помощью python](https://www.rupython.com/nginx-http-python-118239.html)
* [Как настроить аутентификацию на основе токенов для веб-служб с помощью Python?](https://www.rupython.com/110934-110934.html)
* [Как запустить веб-драйвер selenium за прокси-сервером, который нуждается в аутентификации в python](https://www.rupython.com/selenium-x4-5-68153.html)
* [Python: как «разветвить» сеанс в django](https://www.rupython.com/python-django-40-97223.html)

