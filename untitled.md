# Как работать с типизацией в Python

_Рассказывает команда_ [_SimbirSoft_](https://www.simbirsoft.com/?utm_source=tproger&utm_medium=article&utm_campaign=python)

Первые упоминания о подсказках типов в языке программирования Python появились в базе Python Enhancement Proposals \([PEP-483](https://www.python.org/dev/peps/pep-0483/)\). Такие подсказки нужны для улучшения статического анализа кода и автодополнения редакторами, что помогает снизить риски появления багов в коде.

В этой статье мы рассмотрим основы типизации кода Python и ее роль в динамически-типизированном языке, эта информация будет наиболее полезна для начинающих Python-разработчиков.

## Типизация в Python

Для обозначения базовых типов переменных используются сами типы:

* `str`
* `int`
* `float`
* `bool`
* `complex`
* `bytes`
* etc.

Пример использования базовых типов в python-функции:

```text
def func(a: int, b: float) -> str:  
    a: str = f"{a}, {b}"  
    return a
```

Помимо этого, можно параметризировать более сложные типы, например, `List`. Такие типы могут принимать значения параметров, которые помогают более точно описать тип функции. Так, например, `List[int]` указывает на то, что список состоит только из целочисленных значений.

Пример кода:

```text
from typing import List  
  
def func(n: int) -> List[int]:  
    return list(range(n))
```

Кроме `List`, существуют и другие типы из модуля typing, которые можно параметризировать. Такие типы называются Generic-типами. Такого рода типа определены для многих встроенных в Python структур данных:

* `Set[x]`
* `FrozenSet[x]`
* `ByteString[x]`
* `Dict[x, y]`
* `DefaultDict[x, y]`
* `OrderedDict[x, y]`
* `ChainMap[x,y]`
* `Counter[x, int]`
* `Deque[x]`
* и т.д.

Как можно заметить, некоторые типы имеют несколько параметров, которые можно описать. Например, `Dict[x, y]` означает, что это будет словарь, где ключи будут иметь тип `x`, а значения – тип `y`.

Также есть более абстрактные типы, например:

* `Mapping[x, y]` – объект имеет реализации метода `__getitem__`;
* `Iterable[x]` – объект имеет реализацию метода `__iter__`.

При этом функции тоже имеют свои типы. Например, для описания функции можно использовать тип `Callable`, где указываются типы входных параметров и возвращаемых значений. Пример использования:

```text
from typing import Callable  

def func(f: Callable[[int, int], bool]) -> bool:  
    return f(1,2)  
                                                                                
func(lambda x, y: x == y)                                                      
>>> False
```

Тип `Callable`:

* говорит о том, что у объекта реализован метод `__call__`;
* описывает типы параметров к этому методу.

На первом месте стоит массив типов входных параметров, на втором — тип возвращаемого значения.

Про остальные абстрактные типы контейнеров можно [прочитать](https://docs.python.org/3/library/collections.abc.html#collections.abc.Iterable) в документации Python.

Также есть более конкретные типы, например `Literal[x]`, где `x` указывает не тип, а конкретное значение. Например `Literal[3]` означает цифру 3. Используют такой тип крайне редко.

Также Python позволяет определять свои Generic-типы.

```text
from typing import TypeVar, Generic  
     
T = TypeVar('T')  
   
class Stack(Generic[T]):  
    def __init__(self) -> None:  
        # Create an empty list with items of type T  
        self.items: List[T] = []  
   
    def push(self, item: T) -> None:  
        self.items.append(item)  
 
    def pop(self) -> T:  
        return self.items.pop()  

    def empty(self) -> bool:  
        return not self.items
```

В данном примере `TypeVar` означает переменную любого типа, которую можно подставить при указании. Например:

```text
def func(stack: Stack[int]) -> None:  
     stack.push(11)  
     stack.push(-2)  
                                                                                  
s = Stack[int]()                                                               
func(s)                                                                        
s.empty()                                                                      
>>> False

s.items                                                                        
>>> [11, -2]
```

Для определения собственных типов наследование возможно не только от `Generic`, но и от других абстрактных типов, например, таких, как `Mapping`, `Iterable`.

```text
from typing import Generic, TypeVar, Mapping, Iterator, Dict  
   
KeyType = TypeVar('KeyType')  
ValueType = TypeVar('ValueType')  

class MyMap(Mapping[KeyType, ValueType]):  # This is a generic subclass of Mapping  
    def __getitem__(self, k: KeyType) -> ValueType:  
        ...  # Implementations omitted  
    def __iter__(self) -> Iterator[KeyType]:  
        ...  
    def __len__(self) -> int:  
        ...  
```

На месте `KeyType` или `ValueType` могут быть конкретные типы.

Также есть специальные конструкции, которые позволяют комбинировать типы. Например, `Union[x, y, ...]` — один из типов. Если переменной может быть как `int`, так и `float`, то как тип следует указать `Union[int, float]`. Если переменной может быть как `int`, так и `None`**,** то в качестве типа можно указать `Union[int,None]` или, что предпочтительно, `Optional[int]`.

## Зачем это нужно

Цель — указать разработчику на ожидаемый тип данных при получении или возврате данных из функции или метода. В свою очередь, это позволяет сократить количество багов, ускорить написание кода и улучшить его качество.

Допустим, у вас есть класс юзера и функция, которая преобразует json в `User`.

```text
from typing import Dict, Union, Optional                                        

from dataclasses import dataclass                                               

@dataclass  
class User:  
    name: str  
    surname: str  
    age: int                                                                        

def get_user_from_json(json_dict: Dict[str, Optional[Union[int, str]]]) -> User:  
    name = json_dict.get("name")  
    surname = json_dict.get("surname")  
    age = json_dict.get("age")  
    if (age is None or  
        name is None or  
        surname is None):  
        raise ValueError("Not enough information")  
    return User(age=age, name=name, surname=surname)  
```

Конечно, можно написать и проще:

```text
def get_user_from_json(json_dict: Dict[str, Optional[Union[int, str]]]) -> User:  
    return User(age=json_dict["age"], name=json_dict["name"], surname=json_dict["surname"])
```

Однако, в обоих случаях может возникнуть ошибка, если ключ `age` будет присутствовать и при этом иметь строковый тип. Валидация типов добавляет не очень много строк кода, но при большом количестве моделей может занимать немало места в проекте.

Использование [Pydantic](https://pydantic-docs.helpmanual.io/) помогает корректно валидировать данные, при этом тип автоматически поменяется на требуемый.

```text
from pydantic import BaseModel                                                 

class User(BaseModel):  
    name: str  
    surname: str  
    age: int  
                                                                              

def get_user_from_json(json_dict: Dict[str, Optional[Union[int, str]]]) -> User:  
    return User(**json_dict)                                                                               

get_user_from_json({  
    "name": "ssa",  
    "surname": "ddd",  
    "age": 10  
})                                                                             
>>> User(name='ssa', surname='ddd', age=10)

get_user_from_json({  
    "name": "ssa",  
    "surname": "ddd",  
    "age": "10"  
 })                                                                             
>>> User(name='ssa', surname='ddd', age=10)

get_user_from_json({  
    "name": "ssa",  
    "surname": "ddd",  
    "age": "d"  
})
--------------------------------------
ValidationError: 1 validation error for User
age
 value is not a valid integer (type=type_error.integer)                                                                  	

```

Как можно заметить, более строгая типизация кода помогает сделать его проще и безопаснее. Однако, использование некоторых возможностей Pydantic может нежелательно повлиять на код. Так, мутация данных при валидации способна привести к тому, что тип значения модели будет непонятен. Например:

```text
from pydantic import BaseModel, validator                                                   

class User(BaseModel):  
    name: str  
    age: int  

    @validator('age')  
    def validate_age(cls, value):  
        if int(value) < 10:  
            raise ValueError("too low")  
        return str(value)  
                                                                                       
User(name='Brian', age=33)                                                                  
>>> User(name='Brian', age='33')
```

В данном примере созданный User после валидации будет иметь отличный от того, который был указан в модели. Это ведет к возможным крупным багам, которые лучше всегда избегать.

Также сейчас набирает большую популярность фреймворк [FastAPI](https://fastapi.tiangolo.com/), который, благодаря Pydantic, позволяет быстро писать веб-приложения с автоматической валидацией данных.

```text
from fastapi import FastAPI  
from typing import Optional  
from pydantic import BaseModel  
   
app = FastAPI()  

class Item(BaseModel):  
    name: str  
    price: float  
    is_offer: Optional[bool] = None  

@app.put("/item")  
async def put_item(item: Item):  
    return {"item_name": item.name, "item_price": item.price}
```

В данном примере эндпоинт _/item_ автоматически валидирует входящий json и передает его в функцию как требуемую модель.

Также для уменьшения количества багов используют [mypy](http://mypy-lang.org/), который позволяет проводить статический анализ кода на соответствие типов. За счет этого зачастую можно избежать очевидных багов или несоответствий типов в функциях.

И как бонус для тех, кто ленится вручную поддерживать типизацию. [MonkeyType](https://pypi.org/project/MonkeyType/#description) дает возможность автоматически проставить типы во всех функциях, хотя после запуска этой программы обычно требуется пройтись по коду и поправить некоторые значения, которые оказались распознаны не так, как предполагалось.

## Нововведения Python 3.9.0

Начиная с недавно вышедшей версии Python 3.9, у разработчиков больше нет необходимости импортировать абстрактные коллекции для описания типов. Теперь вместо `typing.Dict[x, y]` можно использовать `dict[x,y]`, то же самое происходит с `Deque`, `List`, `Counter` и т.д. Полное описание этого нововведения можно прочитать тут: [PEP-585](https://www.python.org/dev/peps/pep-0585/).

Также добавили аннотации типов, которые в дальнейшем могут быть использованы инструментами статического анализа. `variable: Annotated[T, x]` где `T` — тип переменной `variable`, а `x` — некоторые метаданные для переменной. По оценкам некоторых авторов, эти метаданные могут быть использованы также и во время выполнения \(подробности смотрите в [PEP-593](https://www.python.org/dev/peps/pep-0593/)\).

## Заключение

В этой статье мы рассмотрели некоторые типы в языке Python. В заключение отметим, что типизированный код в Python становится намного более читаемым и очевидным, что помогает проводить ревью в команде и не допускать глупых ошибок. Хорошее описание типов также позволяет разработчикам быстрее влиться в проект, понять, что происходит, и погрузиться в задачи. Также при использовании определенных библиотек удается в несколько раз сократить количество строк кода, которые ранее требовались только для валидации типов и значений.

