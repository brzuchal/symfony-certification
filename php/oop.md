# symfony-certification
## PHP > Object-oriented programming

### Links
[http://php.net/manual/en/language.oop5.php](http://php.net/manual/en/language.oop5.php)

### Info

Starting with PHP 5, the object model was rewritten to allow for better performance and more features. This was a major change from PHP 4. PHP 5 has a full object model.
Among the features in PHP 5 are the inclusions of visibility, abstract and final classes and methods, additional magic methods, interfaces, cloning and typehinting.
PHP treats objects in the same way as references or handles, meaning that each variable contains an object reference rather than a copy of the entire object. See Objects and References

Basic class definitions begin with the keyword `class`, followed by a class name, followed by a pair of curly braces which enclose the definitions of the properties and methods belonging to the class.
The class name can be any valid label, provided it is not a PHP reserved word. A valid class name starts with a letter or underscore, followed by any number of letters, numbers, or underscores. As a regular expression, it would be expressed thus: ^[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*$.
A class may contain its own constants, variables (called "properties"), and functions (called "methods").

The pseudo-variable `$this` is available when a method is called from within an object context. $this is a reference to the calling object (usually the object to which the method belongs, but possibly another object, if the method is called statically from the context of a secondary object).
To create an instance of a class, the new keyword must be used. An object will always be created unless the object has a constructor defined that throws an exception on error. Classes should be defined before instantiation (and in some cases this is a requirement).
If a string containing the name of a class is used with new, a new instance of that class will be created. If the class is in a namespace, its fully qualified name must be used when doing this.

In the class context, it is possible to create a new object by `new self` and `new parent`.
When assigning an already created instance of a class to a new variable, the new variable will access the same instance as the object that was assigned.
This behaviour is the same when passing instances to a function. A copy of an already created object can be made by cloning it.

```php
<?php

$instance = new SimpleClass();

$assigned   =  $instance;
$reference  =& $instance;

$instance->var = '$assigned will have this value';

$instance = null; // $instance and $reference become null

var_dump($instance);
var_dump($reference);
var_dump($assigned);
?>
```

PHP 5.3.0 introduced a couple of new ways to create instances of an object:

```php
<?php
class Test
{
    static public function getNew()
    {
        return new static;
    }
}

class Child extends Test
{}

$obj1 = new Test();
$obj2 = new $obj1;
var_dump($obj1 !== $obj2);

$obj3 = Test::getNew();
var_dump($obj3 instanceof Test);

$obj4 = Child::getNew();
var_dump($obj4 instanceof Child);
?>
```
The above example will output:

> bool(true)
> bool(true)
> bool(true)

A class can inherit the methods and properties of another class by using the keyword `extends` in the class declaration. It is not possible to extend multiple classes; a class can only inherit from one base class.
The inherited methods and properties can be overridden by redeclaring them with the same name defined in the parent class. However, if the parent class has defined a method as `final`, that method may not be overridden. It is possible to access the overridden methods or static properties by referencing them with `parent::`.
**When overriding methods, the parameter signature should remain the same or PHP will generate an E_STRICT level error.** This does not apply to the constructor, which allows overriding with different parameters.

Since PHP 5.5, the `class` keyword is also used for class name resolution. You can get a string containing the fully qualified name of the ClassName class by using `ClassName::class`. This is particularly useful with namespaced classes.


**What is the difference between  $this  and  self ?**

Inside a class definition, $this refers to the current object, while  self  refers to the current class.

It is necessary to refer to a class element using  self ,
and refer to an object element using  $this .
Note also how an object variable must be preceded by a keyword in its definition.