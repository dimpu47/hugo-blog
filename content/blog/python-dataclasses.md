---
title: "Data Classes in Python3.7"
date: 2019-04-07T18:09:49+05:30
draft: true
slug: 'python-dataclasses'
---

### What data.. classes.. ?
Python 3.7 adds a new module in standard library called *Data Classes*. So, what are they? what do they do? how do they benefit ? This article shall cover it all.

Data Classes module provides a [*decorator*](https://www.geeksforgeeks.org/decorators-in-python/) to automate the creation of data models. A dataclass can be thoought of as a mutable object/class that contains no. of fields. These fields are typically known in advance and are stored in a database table or in API. They also have type using a new typing module.


~~~
# Simplest example of dataclasses inntroduced in python3.7
import dataclasses

@dataclasses.dataclass
class MyDataClass(obj):
    field_a: int
    field_b: str
~~~

When reading from database or API, you often want predictable python type to be returned, with a set of expected *fields* and *behaviours*. Dictionaries are a great data structure and very useful to simplify/categorize information. But when you want stadard data model with fixed list of strings, dataclasses are far more powerful utility. 

It also becomes hard to maintain backward-compatibily when you share raw data structures your users. It's common to put logic between accessing and returning of data. This reflection of data from one type to another is a very tedious activity in python. Data Classes are designed as way to automate this reflection bit using decorators involved in creating Data Models.

> dataclasses also leverage type hinting functionality introduced in python3.5

so you can apply type hints to your field in dataclass. This wasn't entirely possible in previous versions of python, it required a lot of boilerplate for leveragin third party packages like _attrs_. 

```
# Long approach
class Customer (object)
    def __init__(self, id: int, name: str, address: str) -> None:
        self.id = id
        self.name = name self.address = address
    def __repr__(self):
        return f'Customer(id={self.id!r}, name={self.name!r}, address={self.address!r})'
    def __eq__(self, other):
        if other.__class__ is self.__class__:
            return (self.id, self.name, self.address) == (other.id, other.name, other.address) return NotImplemented
    def __ne__(self, other):
        if other.__class__ is self.__class__:
            return (self.id, self.name, self.address) != (other.id, other.name, other.address) return NotImplemented
```
Another to achieve similar thing is to use Named Tuples in python. Named tuples are immutable and does not allow attaching methods with it. Any Django developer might think of dataclasses being synonmous to Django Models. Although they are similar, dataclasses is not only for storage in databases.

Below is a generic example of dataclasses, demonstrating a way to store list of customers of a particular website.

~~~
from dataclasses import dataclass, field, InitVar
import typing
import uuid


@dataclass
class Customer(object):
    database: InitVar[typing.Any]
    id: int
    name: str
    address: str

    def __post_init__(self, database):
        self.address = self.address.capitalize()
        self._connection = database.connect()


@dataclass(frozen=True, order=True)
class CustomerOrder(object):
    processing_time: typing.ClassVar[int]

    id: uuid.UUID = field(compare=False, default_factory=uuid.uuid4, init=False)
    value: float = field(compare=True)
    product: str = field(compare=False)


@dataclass(frozen=True)
class CustomerOrderExtended(CustomerOrder):
    date_ordered: str
~~~

>* Mutablity means tendecy to change. 
>* Immutabily is opposite, any type that's not supposed to change, a string constant is perfect example of immutable type.

You can decalare a immutable dataclass that won't change with `frozen=True` attribute. Now if you instantiate CustomerOrder dataclass `order = CustomerOrder(1, 10., "Cheese") ` and try to reassign value of id: `order.id = 2`, it'd throw an error:

    dataclasses.FrozenInstanceError: cannot assign to field id.

This is an expected behaviour. Another big benefit of Frozen Types is that they are hashable by default that means implemented dunder hash magic method: `__hash__`, can be accessed using built-in hash method:

    hash(order)
    -376120387594030284

Hashable types can be used in sets and can also be used as a key in a dictionary, so if you wanted a dictionary of orders and it's statuses, you could create a dictionary:

    order_statuses = { order: 'processed' }
    {CustomerOrder(id=1, value=10.0, product='cheese'): 'processed'}

> Hashable types are implictly implemented when the type is immutable.

dunder magic hash method is appplied when when dataclass is frozen. It can also be applied to mutable dataclasses with `hash=True` attribute passed in: 
    
    @dataclass(hash=True)
    class Customer(object):
        id: int
        name: str
        address: str