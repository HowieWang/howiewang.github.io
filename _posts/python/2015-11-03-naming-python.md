---
layout: post
title:  python命名规则-官方推荐
category: 
- python  

---

* content
{:toc}

## Prescriptive: Naming Conventions

### Names to Avoid
Never use the characters `'l' (lowercase letter el), 'O' (uppercase letter oh), or 'I' (uppercase letter eye)` as single character variable names.

In some fonts, these characters are indistinguishable from the numerals one and zero. `When tempted to use 'l', use 'L' instead.`

### Package and Module Names
`Modules should have short, all-lowercase names. `Underscores can be used in the module name if it improves readability. Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.

When an extension module written in C or C++ has an accompanying Python module that provides a higher level (e.g. more object oriented) interface, the C/C++ module has a leading underscore (e.g. _socket ).

### Class Names
Class names should normally use the `CapWords` convention.

The naming convention for functions may be used instead in cases where the interface is documented and used primarily as a callable.

Note that there is a separate convention for builtin names: most builtin names are single words (or two words run together), with the CapWords convention used only for exception names and builtin constants.

### Exception Names
Because exceptions should be classes, the class naming convention applies here. However, you should use the suffix "Error" on your exception names (if the exception actually is an error).

### Global Variable Names
(Let's hope that these variables are meant for use inside one module only.) The conventions are about the same as those for functions.

Modules that are designed for use via from M import * should use the __all__ mechanism to prevent exporting globals, or use the older convention of prefixing such globals with an underscore (which you might want to do to indicate these globals are "module non-public").

### Function Names
Function names should be `lowercase`, with words `separated by underscores `as necessary to improve readability.

mixedCase is allowed only in contexts where that's already the prevailing style (e.g. threading.py), to retain backwards compatibility.

### Function and method arguments
Always `use self for the first argument` to instance methods.

Always `use cls for the first argument to class methods`.

If a function argument's name clashes with a reserved keyword, it is generally better to append a single trailing underscore rather than use an abbreviation or spelling corruption. Thus class_ is better than clss . (Perhaps better is to avoid such clashes by using a synonym.)

### Method Names and Instance Variables
Use the function naming rules:` lowercase with words separated by underscores `as necessary to improve readability.

Use `one leading underscore` only for `non-public methods and instance variables`.

To avoid name clashes with subclasses, `use two leading underscores to invoke Python's name mangling rules`.

Python mangles these names with the class name: if class Foo has an attribute named __a , it cannot be accessed by Foo.__a . (An insistent user could still gain access by calling Foo._Foo__a .) Generally, double leading underscores should be used only to avoid name conflicts with attributes in classes designed to be subclassed.

Note: there is some controversy about the use of __names (see below).

### Constants
Constants are usually defined on a module level and written in `all capital letters `with underscores separating words. Examples include MAX_OVERFLOW and TOTAL .

### Designing for inheritance
Always decide whether a class's methods and instance variables (collectively: "attributes") should be public or non-public. If in doubt, choose non-public; it's easier to make it public later than to make a public attribute non-public.

Public attributes are those that you expect unrelated clients of your class to use, with your commitment to avoid backward incompatible changes. Non-public attributes are those that are not intended to be used by third parties; you make no guarantees that non-public attributes won't change or even be removed.

We don't use the term "private" here, since no attribute is really private in Python (without a generally unnecessary amount of work).

Another category of attributes are those that are part of the "subclass API" (often called "protected" in other languages). Some classes are designed to be inherited from, either to extend or modify aspects of the class's behavior. When designing such a class, take care to make explicit decisions about which attributes are public, which are part of the subclass API, and which are truly only to be used by your base class.

With this in mind, here are the Pythonic guidelines:

Public attributes should have no leading underscores.

If your public attribute name collides with a reserved keyword, append a single trailing underscore to your attribute name. This is preferable to an abbreviation or corrupted spelling. (However, notwithstanding this rule, 'cls' is the preferred spelling for any variable or argument which is known to be a class, especially the first argument to a class method.)

> Note 1: See the argument name recommendation above for class methods.

For simple public data attributes, it is best to expose just the attribute name, without complicated accessor/mutator methods. Keep in mind that Python provides an easy path to future enhancement, should you find that a simple data attribute needs to grow functional behavior. In that case, use properties to hide functional implementation behind simple data attribute access syntax.

> Note 1: Properties only work on new-style classes.

> Note 2: Try to keep the functional behavior side-effect free, although side-effects such as caching are generally fine.

> Note 3: Avoid using properties for computationally expensive operations; the attribute notation makes the caller believe that access is (relatively) cheap.

If your class is intended to be subclassed, and you have attributes that you do not want subclasses to use, consider naming them with double leading underscores and no trailing underscores. This invokes Python's name mangling algorithm, where the name of the class is mangled into the attribute name. This helps avoid attribute name collisions should subclasses inadvertently contain attributes with the same name.

> Note 1: Note that only the simple class name is used in the mangled name, so if a subclass chooses both the same class name and attribute name, you can still get name collisions.

> Note 2: Name mangling can make certain uses, such as debugging and `__getattr__() `, less convenient. However the name mangling algorithm is well documented and easy to perform manually.

> Note 3: Not everyone likes name mangling. Try to balance the need to avoid accidental name clashes with potential use by advanced callers.


[find detail here](https://www.python.org/dev/peps/pep-0008/#function-names)
