# symfony-certification
## PHP > Object-oriented programming

### Links
[http://php.net/manual/en/language.oop5.php](http://php.net/manual/en/language.oop5.php)

### Basics

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

### Properties

Class member variables are called "properties". You may also see them referred to using other terms such as "attributes" or "fields", but for the purposes of this reference we will use "properties". They are defined by using one of the keywords `public`, `protected`, or `private`, followed by a normal variable declaration. This declaration may include an initialization, but this initialization must be a constant value--that is, it must be able to be evaluated at compile time and must not depend on run-time information in order to be evaluated.
```
In order to maintain backward compatibility with PHP 4, PHP 5 will still accept the use of the keyword var in property declarations instead of (or in addition to) public, protected, or private. However, var is no longer required. In versions of PHP from 5.0 to 5.1.3, the use of var was considered deprecated and would issue an E_STRICT warning, but since PHP 5.1.3 it is no longer deprecated and does not issue the warning.
If you declare a property using var instead of one of public, protected, or private, then PHP 5 will treat the property as if it had been declared as public
```

Within class methods non-static properties may be accessed by using `->` (Object Operator): `$this->property` (where property is the name of the property). Static properties are accessed by using the `::` (Double Colon): `self::$property`.

```php
<?php
class SimpleClass
{
   // invalid property declarations:
   public $var1 = 'hello ' . 'world';
   public $var2 = <<<EOD
hello world
EOD;
   public $var3 = 1+2;
   public $var4 = self::myStaticMethod();
   public $var5 = $myVar;

   // valid property declarations:
   public $var6 = myConstant;
   public $var7 = array(true, false);

   // This is allowed only in PHP 5.3.0 and later.
   public $var8 = <<<'EOD'
hello world
EOD;
}
?>
```

### Constants

It is possible to define constant values on a per-class basis remaining the same and unchangeable. Constants differ from normal variables in that you don't use the $ symbol to declare or use them.

The value must be a constant expression, not (for example) a variable, a property, or a function call.

*It's also possible for interfaces to have constants.*

As of PHP 5.3.0, it's possible to reference the class using a variable. The variable's value can not be a keyword (e.g. self, parent and static).
```php
<?php
class MyClass
{
    const CONSTANT = 'constant value';

    function showConstant() {
        echo  self::CONSTANT . "\n";
    }
}

echo MyClass::CONSTANT . "\n";

$classname = "MyClass";
echo $classname::CONSTANT . "\n"; // As of PHP 5.3.0

$class = new MyClass();
$class->showConstant();

echo $class::CONSTANT."\n"; // As of PHP 5.3.0
?>
```

Static data example
```php
<?php
class foo {
    // As of PHP 5.3.0
    const BAR = <<<'EOT'
bar
EOT;
    // As of PHP 5.3.0
    const BAZ = <<<EOT
baz
EOT;
}
?>
```
Constant expression example
```php
<?php
const ONE = 1;

class foo {
    // As of PHP 5.6.0
    const TWO = ONE * 2;
    const THREE = ONE + self::TWO;
    const SENTENCE = 'The value of THREE is '.self::THREE;
}
?>
```
It is possible to provide a scalar expression involving numeric and string literals and/or constants in context of a class constant.

### Autoloading

Many developers writing object-oriented applications create one PHP source file per class definition. One of the biggest annoyances is having to write a long list of needed includes at the beginning of each script (one for each class).

In PHP 5, this is no longer necessary. You may define an `__autoload()` function which is automatically called in case you are trying to use a class/interface which hasn't been defined yet. By calling this function the scripting engine is given a last chance to load the class before PHP fails with an error.
`spl_autoload_register()` provides a more flexible alternative for autoloading classes. For this reason, using `__autoload()` is discouraged and may be deprecated or removed in the future.

### Constructors

PHP 5 allows developers to declare constructor methods for classes. Classes which have a constructor method call this method on each newly-created object, so it is suitable for any initialization that the object may need before it is used.
Note: Parent constructors are not called implicitly if the child class defines a constructor. In order to run a parent constructor, a call to `parent::__construct()` within the child constructor is required. If the child does not define a constructor then it may be inherited from the parent class just like a normal class method (if it was not declared as private).

```php
<?php
class BaseClass {
   function __construct() {
       print "In BaseClass constructor\n";
   }
}

class SubClass extends BaseClass {
   function __construct() {
       parent::__construct();
       print "In SubClass constructor\n";
   }
}

class OtherSubClass extends BaseClass {
    // inherits BaseClass's constructor
}

// In BaseClass constructor
$obj = new BaseClass();

// In BaseClass constructor
// In SubClass constructor
$obj = new SubClass();

// In BaseClass constructor
$obj = new OtherSubClass();
?>
```

For backwards compatibility with PHP 3 and 4, if PHP cannot find a __construct() function for a given class, and the class did not inherit one from a parent class, it will search for the old-style constructor function, by the name of the class. Effectively, it means that the only case that would have compatibility issues is if the class had a method named __construct() which was used for different semantics.

Warning
Old style constructors are DEPRECATED in PHP 7.0, and will be removed in a future version. You should always use __construct() in new code.
Unlike with other methods, PHP will not generate an E_STRICT level error message when `__construct()` is overridden with different parameters than the parent `__construct()` method has.

As of PHP 5.3.3, methods with the same name as the last element of a namespaced class name will no longer be treated as constructor. This change doesn't affect non-namespaced classes.

Constructors in namespaced classes
```php
<?php
namespace Foo;
class Bar {
    public function Bar() {
        // treated as constructor in PHP 5.3.0-5.3.2
        // treated as regular method as of PHP 5.3.3
    }
}
?>
```
### Destructors

PHP 5 introduces a destructor concept similar to that of other object-oriented languages, such as C++. The destructor method will be called as soon as there are no other references to a particular object, or in any order during the shutdown sequence.

Like constructors, parent destructors will not be called implicitly by the engine. In order to run a parent destructor, one would have to explicitly call `parent::__destruct()` in the destructor body. Also like constructors, a child class may inherit the parent's destructor if it does not implement one itself.

The destructor will be called even if script execution is stopped using `exit()`. Calling `exit()` in a destructor will prevent the remaining shutdown routines from executing.

**Note:
Destructors called during the script shutdown have HTTP headers already sent. The working directory in the script shutdown phase can be different with some SAPIs (e.g. Apache).**

**Note:
Attempting to throw an exception from a destructor (called in the time of script termination) causes a fatal error.**

### Visibility

The visibility of a property or method can be defined by prefixing the declaration with the keywords `public`, `protected` or `private`.
Class members declared `public` can be accessed everywhere.
Members declared `protected` can be accessed only within the class itself and by inherited classes.
Members declared as `private` may only be accessed by the class that defines the member.
```php
<?php
/**
 * Define MyClass
 */
class MyClass
{
    public $public = 'Public';
    protected $protected = 'Protected';
    private $private = 'Private';

    function printHello()
    {
        echo $this->public;
        echo $this->protected;
        echo $this->private;
    }
}

$obj = new MyClass();
echo $obj->public; // Works
echo $obj->protected; // Fatal Error
echo $obj->private; // Fatal Error
$obj->printHello(); // Shows Public, Protected and Private


/**
 * Define MyClass2
 */
class MyClass2 extends MyClass
{
    // We can redeclare the public and protected method, but not private
    protected $protected = 'Protected2';

    function printHello()
    {
        echo $this->public;
        echo $this->protected;
        echo $this->private;
    }
}

$obj2 = new MyClass2();
echo $obj2->public; // Works
echo $obj2->protected; // Fatal Error
echo $obj2->private; // Undefined
$obj2->printHello(); // Shows Public, Protected2, Undefined

?>
```

**Objects of the same type will have access to each others private and protected members even though they are not the same instances. This is because the implementation specific details are already known when inside those objects.**
```php
<?php
class Test
{
    private $foo;

    public function __construct($foo)
    {
        $this->foo = $foo;
    }

    private function bar()
    {
        echo 'Accessed the private method.';
    }

    public function baz(Test $other)
    {
        // We can change the private property:
        $other->foo = 'hello';
        var_dump($other->foo);

        // We can also call the private method:
        $other->bar();
    }
}

$test = new Test('test');

$test->baz(new Test('other'));
?>
```

The manual says that "Private limits visibility only to the class that defines the item". That means extended children classes do not see the private methods of parent class and vice versa also.

As a result, parents and children can have different implementations of the "same" private methods, depending on where you call them (e.g. parent or child class instance). Why? Because private methods are visible only for the class that defines them and the child class does not see the parent's private methods. If the child doesn't see the parent's private methods, the child can't override them. Scopes are different. In other words -- each class has a private set of private variables that no-one else has access to.

A sample demonstrating the percularities of private methods when extending classes:
```php
<?php
abstract class base {
    public function inherited() {
        $this->overridden();
    }
    private function overridden() {
        echo 'base';
    }
}

class child extends base {
    private function overridden() {
        echo 'child';
    }
}

$test = new child();
$test->inherited();
?>
```
Output will be "base".

If you want the inherited methods to use overridden functionality in extended classes but public sounds too loose, use protected. That's what it is for:)

A sample that works as intended:
```php
<?php
abstract class base {
    public function inherited() {
        $this->overridden();
    }
    protected function overridden() {
        echo 'base';
    }
}

class child extends base {
    protected function overridden() {
        echo 'child';
    }
}

$test = new child();
$test->inherited();
?>
```
Output will be "child".

A class A static public function can access to class A private function :
```php
<?php
class A {
    private function foo()
    {
        print("bar");
    }

    static public function bar($a)
    {
        $a->foo();
    }
}

$a = new A();

A::bar($a);
?>

It's working.
```