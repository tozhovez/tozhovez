# Python Language - py.test \| python Tutorial

## Настройка py.test <a id="----------py-test"></a>

 `py.test` - одна из нескольких [сторонних библиотек тестирования](http://docs.pytest.org/en/latest/) , доступных для Python. Он может быть установлен с помощью [`pip`](https://sodocumentation.net/fr/python/topic/1781/pip--pypi-package-manager) с

```text
pip install pytest
```

##  Код для тестирования

 Скажем, мы тестируем функцию добавления в `projectroot/module/code.py` :

```text
# projectroot/module/code.py
def add(a, b):
    return a + b
```

##  Тестирование

 Мы создаем тестовый файл в `projectroot/tests/test_code.py` . Файл **должен начинаться с `test_`** для распознавания в качестве файла тестирования.

```text
# projectroot/tests/test_code.py
from module import code


def test_add():
    assert code.add(1, 2) == 3
```

##  Выполнение теста

 Из `projectroot` мы просто запускаем `py.test` :

```text
# ensure we have the modules
$ touch tests/__init__.py
$ touch module/__init__.py
$ py.test
================================================== test session starts ===================================================
platform darwin -- Python 2.7.10, pytest-2.9.2, py-1.4.31, pluggy-0.3.1
rootdir: /projectroot, inifile:
collected 1 items

tests/test_code.py .

================================================ 1 passed in 0.01 seconds ================================================
```

## Неудачные тесты <a id="---------------"></a>

 Пробный тест даст полезную информацию о том, что пошло не так:

```text
# projectroot/tests/test_code.py
from module import code


def test_add__failing():
    assert code.add(10, 11) == 33
```

 Результаты:

```text
$ py.test
================================================== test session starts ===================================================
platform darwin -- Python 2.7.10, pytest-2.9.2, py-1.4.31, pluggy-0.3.1
rootdir: /projectroot, inifile:
collected 1 items

tests/test_code.py F

======================================================== FAILURES ========================================================
___________________________________________________ test_add__failing ____________________________________________________

    def test_add__failing():
>       assert code.add(10, 11) == 33
E       assert 21 == 33
E        +  where 21 = (10, 11)
E        +    where  = code.add

tests/test_code.py:5: AssertionError
================================================ 1 failed in 0.01 seconds ================================================
```

## Введение в тестовые светильники <a id="-------------------------------"></a>

 Более сложные тесты иногда должны иметь настройки, прежде чем запускать код, который вы хотите проверить. Это можно сделать в самой тестовой функции, но тогда вы оказываете большие функции тестирования, делая так много, что трудно сказать, где остановится установка, и начинается тест. Вы также можете получить много дублирующего кода установки между различными функциями тестирования.

 Наш файл кода:

```text
# projectroot/module/stuff.py
class Stuff(object):
    def prep(self):
        self.foo = 1
        self.bar = 2
```

 Наш тестовый файл:

```text
# projectroot/tests/test_stuff.py
import pytest
from module import stuff


def test_foo_updates():
    my_stuff = stuff.Stuff()
    my_stuff.prep()
    assert 1 == my_stuff.foo
    my_stuff.foo = 30000
    assert my_stuff.foo == 30000


def test_bar_updates():
    my_stuff = stuff.Stuff()
    my_stuff.prep()
    assert 2 == my_stuff.bar
    my_stuff.bar = 42
    assert 42 == my_stuff.bar
```

 Это довольно простые примеры, но если нашему объекту `Stuff` понадобилось гораздо больше настроек, оно получило бы громоздкий характер. Мы видим, что между нашими тестовыми примерами существует дублированный код, поэтому давайте сначала переформулируем его в отдельную функцию.

```text
# projectroot/tests/test_stuff.py
import pytest
from module import stuff


def get_prepped_stuff():
    my_stuff = stuff.Stuff()
    my_stuff.prep()
    return my_stuff


def test_foo_updates():
    my_stuff = get_prepped_stuff()
    assert 1 == my_stuff.foo
    my_stuff.foo = 30000
    assert my_stuff.foo == 30000


def test_bar_updates():
    my_stuff = get_prepped_stuff()
    assert 2 == my_stuff.bar
    my_stuff.bar = 42
    assert 42 == my_stuff.bar
```

 Это выглядит лучше, но у нас все еще есть `my_stuff = get_prepped_stuff()` загромождающий наши тестовые функции.

##  py.test приспособления на помощь!

 Светильники - это гораздо более мощные и гибкие версии функций тестовой настройки. Они могут сделать намного больше, чем мы используем здесь, но мы будем делать это шаг за шагом.

 Сначала мы меняем `get_prepped_stuff` на приспособление, называемое `prepped_stuff` . Вы хотите называть свои светильники именами, а не глаголами, из-за того, как приборы впоследствии будут использоваться в самих тестовых функциях. Параметр `@pytest.fixture` указывает, что эту конкретную функцию следует обрабатывать как привязку, а не как обычную функцию.

```text
@pytest.fixture
def prepped_stuff():
    my_stuff = stuff.Stuff()
    my_stuff.prep()
    return my_stuff
```

 Теперь мы должны обновить тестовые функции, чтобы они использовали прибор. Это делается путем добавления параметра к определению, которое точно соответствует имени прибора. Когда py.test выполняется, он запускает прибор перед запуском теста, а затем передает возвращаемое значение прибора в тестовую функцию через этот параметр. \(Обратите внимание, что светильники не **должны** возвращать значение, они могут делать другие вещи настройки, например, вызывать внешний ресурс, упорядочивать вещи в файловой системе, помещать значения в базу данных, независимо от того, какие тесты необходимы для настройки\)

```text
def test_foo_updates(prepped_stuff):
    my_stuff = prepped_stuff
    assert 1 == my_stuff.foo
    my_stuff.foo = 30000
    assert my_stuff.foo == 30000


def test_bar_updates(prepped_stuff):
    my_stuff = prepped_stuff
    assert 2 == my_stuff.bar
    my_stuff.bar = 42
    assert 42 == my_stuff.bar
```

 Теперь вы можете понять, почему мы назвали его существительным. но `my_stuff = prepped_stuff` практически бесполезна, поэтому давайте просто использовать `prepped_stuff` прямо.

```text
def test_foo_updates(prepped_stuff):
    assert 1 == prepped_stuff.foo
    prepped_stuff.foo = 30000
    assert prepped_stuff.foo == 30000


def test_bar_updates(prepped_stuff):
    assert 2 == prepped_stuff.bar
    prepped_stuff.bar = 42
    assert 42 == prepped_stuff.bar
```

 Теперь мы используем светильники! Мы можем пойти дальше, изменив область действия прибора \(так что он запускается только один раз на тестовый модуль или сеанс выполнения тестового набора, а не один раз на каждую тестовую функцию\), создавая светильники, которые используют другие приборы, параметризуя прибор \(чтобы светильник и все тесты, использующие этот прибор, выполняются несколько раз, один раз для каждого параметра, присвоенного прибору\), приборы, которые считывают значения из модуля, который их вызывает ..., как упоминалось ранее, светильники обладают гораздо большей мощностью и гибкостью, чем обычная функция настройки.

##  Очистка после проведения тестов.

 Скажем, наш код вырос, и нашему объекту Stuff теперь нужна специальная очистка.

```text
# projectroot/module/stuff.py
class Stuff(object):
def prep(self):
    self.foo = 1
    self.bar = 2

def finish(self):
    self.foo = 0
    self.bar = 0
```

 Мы могли бы добавить код для вызова очистки в нижней части каждой тестовой функции, но приспособления обеспечивают лучший способ сделать это. Если вы добавите функцию в прибор и зарегистрируете его в качестве **финализатора** , код в функции финализатора будет вызван после теста с использованием приспособления. Если объем устройства больше одной функции \(например, модуля или сеанса\), финализатор будет выполнен после завершения всех тестов в области видимости, поэтому после завершения работы модуля или в конце всего сеанса тестирования ,

```text
@pytest.fixture
def prepped_stuff(request):  # we need to pass in the request to use finalizers
    my_stuff = stuff.Stuff()
    my_stuff.prep()
    def fin():  # finalizer function
        # do all the cleanup here
        my_stuff.finish()
    request.addfinalizer(fin)  # register fin() as a finalizer
    # you can do more setup here if you really want to
    return my_stuff
```

 Использование функции финализатора внутри функции может быть немного сложно понять на первый взгляд, особенно когда у вас есть более сложные приборы. Вместо этого вы можете использовать **приспособление** с **урожаем,** чтобы сделать то же самое с более понятным для пользователя потоком выполнения. Единственное реальное отличие заключается в том, что вместо использования `return` мы используем `yield` в той части прибора, где выполняется настройка, и управление должно перейти к тестовой функции, а затем добавить весь код очистки после `yield` . Мы также украшаем его как `yield_fixture` так что py.test знает, как с ним справиться.

```text
@pytest.yield_fixture
def prepped_stuff():  # it doesn't need request now!
    # do setup
    my_stuff = stuff.Stuff()
    my_stuff.prep()
    # setup is done, pass control to the test functions
    yield my_stuff
    # do cleanup 
    my_stuff.finish()
```

 И это завершает Intro для тестирования светильников!

 Для получения дополнительной информации см. [Официальную документацию по документации](http://doc.pytest.org/en/latest/fixture.html) на [py.test](http://doc.pytest.org/en/latest/fixture.html) и [официальную документацию по инвентаризации](http://doc.pytest.org/en/latest/yieldfixture.html)  
  


