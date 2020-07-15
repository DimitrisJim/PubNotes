---
layout: post
title: Conforming to ABCs
tags: python, abc
---

The [PEP][AbcPEP] does a great job of summarizing the rational and giving a first introduction to the changes. The main rational, quoting from the PEP:

> This PEP proposes a particular strategy for organizing these tests known as Abstract Base Classes, or ABC. ABCs are simply Python classes that are added into an object's inheritance tree to signal certain features of that object to an external inspector.

I just want to talk about the specific way this organization can happen. As far as I could understand, there's three ways:

 - Subclass a given ABC.
 - Register the ABC as a virtual superclass.
 - Implement methods defined in `__subclasshook__`.

 Each of these has some subtle differences, they all achieve the same goal though: allowing you to answer the question "I implement this set of methods."

### Subclass an ABC

Subclassing an ABC can be done not only in order to conform to a given set of
features but also to get some methods for free. `Set`, for example, implements
union, intersection et al for you in a reasonable default way; you don't need
to implement these during your first iteration of implementing a set like class.

Of course, with the free methods you get, you must also implement any `abstractmethods` this class defines. In a sense, this is the most strict way of utilizing ABCs: Python checks that you implement the required methods by not allowing you to instantiate your class otherwise.

### Using ABCMeta.register

Using the `register` method for your type simply means: I am telling you I
implement the things you need, trust me.

This way doesn't add any of the mixin methods from the ABC to your class and you also aren't required to implement any of the `abstractmethods` defined. If you aren't utilizing the language as a consenting adult, you'll get bitten.

### Implement methods defined in `__subclasshook__`

These *usually* correspond to the `abstractmethods` an ABC defines. This isn't quite the same as `register` since using `register` won't check that you have
a specific set of methods implemented. When an ABC implements `__subclasshook__`
it will go through the methods you defined and check if you have implemented
the required methods, if not, you get `False` with `isinstance` and `issubclass`. As an example:

```python
from collections.abc import Container

class Spam:
    pass

print(isinstance(Spam(), Container))  # False
print(issubclass(Spam, Container))    # False

Container.register(Spam)  # not the best idea, given Spam's implementation

print(isinstance(Spam(), Container))  # True
print(issubclass(Spam, Container))    # True
```





[AbcPEP]: https://www.python.org/dev/peps/pep-3119/
