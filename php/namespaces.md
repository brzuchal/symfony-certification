# symfony-certification
## PHP > Namespaces

### Links
[http://php.net/manual/en/language.namespaces.php](http://php.net/manual/en/language.namespaces.php)

### Overview
**(PHP 5 >= 5.3.0)**

What are namespaces? In the broadest definition namespaces are a way of encapsulating items. This can be seen as an abstract concept in many places. For example, in any operating system directories serve to group related files, and act as a namespace for the files within them. As a concrete example, the file foo.txt can exist in both directory /home/greg and in /home/other, but two copies of foo.txt cannot co-exist in the same directory. In addition, to access the foo.txt file outside of the /home/greg directory, we must prepend the directory name to the file name using the directory separator to get /home/greg/foo.txt. This same principle extends to namespaces in the programming world.

In the PHP world, namespaces are designed to solve two problems that authors of libraries and applications encounter when creating re-usable code elements such as classes or functions:

* Name collisions between code you create, and internal PHP classes/functions/constants or third-party classes/functions/constants.
* Ability to alias (or shorten) Extra_Long_Names designed to alleviate the first problem, improving readability of source code.

Note:
**Namespace names PHP and php, and compound names starting with these names (like PHP\Classes) are reserved for internal language use and should not be used in the userspace code.**

Just a note: namespace (even nested or sub-namespace) cannot be just a number, it must start with a letter.
For example, lets say you want to use namespace for versioning of your packages or versioning of your API:
```php
namespace Mynamespace\1;  // Illegal
```
Instead use this:
```php
namespace Mynamespace\v1; // OK
```

**php namespaces are case-insensitive**

### Defining namespaces

Although any valid PHP code can be contained within a namespace, only the following types of code are affected by namespaces: **classes** (including abstracts and traits), **interfaces**, **functions** and **constants**.

Namespaces are declared using the `namespace` keyword. **A file containing a namespace must declare the namespace at the top of the file before any other code - with one exception: the declare keyword.**

Regarding constants defined with define() inside namespaces...

define() will define constants exactly as specified.  So, if you want to define a constant in a namespace, you will need to specify the namespace in your call to define(), even if you're calling define() from within a namespace.  The following examples will make it clear.

The following code will define the constant "MESSAGE" in the global namespace (i.e. "\MESSAGE").
```php
<?php
namespace test;
define('MESSAGE', 'Hello world!');
?>
```

The following code will define two constants in the "test" namespace.
```php
<?php
namespace test;
define('test\HELLO', 'Hello world!');
define(__NAMESPACE__ . '\GOODBYE', 'Goodbye cruel world!');
?>
```
```php
<?php
    namespace NS;

    define(__NAMESPACE__ .'\foo','111');
    define('foo','222');

    echo foo;  // 111.
    echo \foo;  // 222.
    echo \NS\foo  // 111.
    echo NS\foo  // fatal error. assumes \NS\NS\foo.
?>
```
Much like directories and files, PHP namespaces also contain the ability to specify a hierarchy of namespace names. Thus, a namespace name can be defined with sub-levels:

Multiple namespaces may also be declared in the same file. There are two allowed syntaxes.
```php
<?php
namespace MyProject;

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }

namespace AnotherProject;

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
?>
```
This syntax is not recommended for combining namespaces into a single file. Instead it is recommended to use the alternate bracketed syntax.
```php
<?php
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace AnotherProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}
?>
```

It is strongly discouraged as a coding practice to combine multiple namespaces into the same file. The primary use case is to combine multiple PHP scripts into the same file.

To combine global non-namespaced code with namespaced code, only bracketed syntax is supported. Global code should be encased in a namespace statement with no namespace name as in:
```php
<?php
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // global code
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```

No PHP code may exist outside of the namespace brackets except for an opening declare statement.

"use" statements are required to be placed after the "namespace my\space" but before the "{".
e.g.
```php
<?php
namespace foo\bar;
use my\space\MyClass;
{

// place code here

} // end of namespace foo\bar

namespace another\bar;
use my\space\MyClass;
use my\space\AnotherClass;
{

// place code here

} // end of namespace another\bar
?>
```

### Basics

(PHP 5 >= 5.3.0)
Before discussing the use of namespaces, it is important to understand how PHP knows which namespaced element your code is requesting. A simple analogy can be made between PHP namespaces and a filesystem. There are three ways to access a file in a file system:

* Relative file name like foo.txt. This resolves to currentdirectory/foo.txt where currentdirectory is the directory currently occupied. So if the current directory is /home/foo, the name resolves to /home/foo/foo.txt.
* Relative path name like subdirectory/foo.txt. This resolves to currentdirectory/subdirectory/foo.txt.
* Absolute path name like /main/foo.txt. This resolves to /main/foo.txt.

The same principle can be applied to namespaced elements in PHP. For example, a class name can be referred to in three ways:
* Unqualified name, or an unprefixed class name like $a = new foo(); or foo::staticmethod();. If the current namespace is currentnamespace, this resolves to currentnamespace\foo. If the code is global, non-namespaced code, this resolves to foo. One caveat: unqualified names for functions and constants will resolve to global functions and constants if the namespaced function or constant is not defined. See Using namespaces: fallback to global function/constant for details.
* Qualified name, or a prefixed class name like $a = new subnamespace\foo(); or subnamespace\foo::staticmethod();. If the current namespace is currentnamespace, this resolves to currentnamespace\subnamespace\foo. If the code is global, non-namespaced code, this resolves to subnamespace\foo.
* Fully qualified name, or a prefixed name with global prefix operator like $a = new \currentnamespace\foo(); or \currentnamespace\foo::staticmethod();. This always resolves to the literal name specified in the code, currentnamespace\foo.

### Namespaces and dynamic language features

PHP's implementation of namespaces is influenced by its dynamic nature as a programming language. Thus, to convert code like the following example into namespaced code:
ple1.php:
```php
<?php
class classname
{
    function __construct()
    {
        echo __METHOD__,"\n";
    }
}
function funcname()
{
    echo __FUNCTION__,"\n";
}
const constname = "global";

$a = 'classname';
$obj = new $a; // prints classname::__construct
$b = 'funcname';
$b(); // prints funcname
echo constant('constname'), "\n"; // prints global
?>
```
One must use the fully qualified name (class name with namespace prefix). Note that because there is no difference between a qualified and a fully qualified Name inside a dynamic class name, function name, or constant name, the leading backslash is not necessary.

```php
<?php
namespace namespacename;
class classname
{
    function __construct()
    {
        echo __METHOD__,"\n";
    }
}
function funcname()
{
    echo __FUNCTION__,"\n";
}
const constname = "namespaced";

include 'example1.php';

$a = 'classname';
$obj = new $a; // prints classname::__construct
$b = 'funcname';
$b(); // prints funcname
echo constant('constname'), "\n"; // prints global

/* note that if using double quotes, "\\namespacename\\classname" must be used */
$a = '\namespacename\classname';
$obj = new $a; // prints namespacename\classname::__construct
$a = 'namespacename\classname';
$obj = new $a; // also prints namespacename\classname::__construct
$b = 'namespacename\funcname';
$b(); // prints namespacename\funcname
$b = '\namespacename\funcname';
$b(); // also prints namespacename\funcname
echo constant('\namespacename\constname'), "\n"; // prints namespaced
echo constant('namespacename\constname'), "\n"; // also prints namespaced
?>
```
### namespace keyword and __NAMESPACE__ constant

PHP supports two ways of abstractly accessing elements within the current namespace, the `__NAMESPACE__` magic constant, and the `namespace` keyword.

The value of `__NAMESPACE__` is a string that contains the current namespace name. In global, un-namespaced code, it contains an empty string.
```php
<?php
namespace MyProject;

echo '"', __NAMESPACE__, '"'; // outputs "MyProject"
?>```

```php
<?php
namespace MyProject;

use blah\blah as mine; // see "Using namespaces: Aliasing/Importing"

blah\mine(); // calls function MyProject\blah\mine()
namespace\blah\mine(); // calls function MyProject\blah\mine()

namespace\func(); // calls function MyProject\func()
namespace\sub\func(); // calls function MyProject\sub\func()
namespace\cname::method(); // calls static method "method" of class MyProject\cname
$a = new namespace\sub\cname(); // instantiates object of class MyProject\sub\cname
$b = namespace\CONSTANT; // assigns value of constant MyProject\CONSTANT to $b
?>

Just in case you wonder what the practical use of the namespace keyword is...

It can explicitly refer to classes from the current namespace regardless of possibly "use"d classes with the same name from other namespaces. However, this does not apply for functions.

Example:
```php
<?php
namespace foo;
class Xyz {}
function abc () {}
?>

<?php
namespace bar;
class Xyz {}
function abc () {}
?>

<?php
namespace bar;
use foo\Xyz;
use foo\abc;
new Xyz(); // instantiates \foo\Xyz
new namespace\Xyz(); // instantiates \bar\Xyz
abc(); // invokes \bar\abc regardless of the second use statement
\foo\abc(); // it has to be invoked using the fully qualified name
?>
```

### Using namespaces: Aliasing/Importing

The ability to refer to an external fully qualified name with an alias, or importing, is an important feature of namespaces. This is similar to the ability of unix-based filesystems to create symbolic links to a file or to a directory.

All versions of PHP that support namespaces support three kinds of aliasing or importing: aliasing a class name, aliasing an interface name, and aliasing a namespace name. PHP 5.6+ also allows aliasing or importing function and constant names.

