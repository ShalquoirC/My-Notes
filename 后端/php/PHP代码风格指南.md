# PHP编码规范

## PSR-2

### 概要

  * 务必要遵循 [PSR-1](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md) 规范；
  * 4个空格缩进（编辑器设置下Tab键占4个空格即可）；
  * 每行字符不得超过`120`个，最好控制在`80`内；
  * 声明`namespace`后空一行；
  * 声明`use`后空一行，多个`use`之间不空行；
  * `class`与`method`的开始`{`与结束`}`括号必须独自占一行；
  * `class`的属性与方法可见性必须注明，且`abstract` 、`final`必须在`public`等一般可见性之前，`static`排最后。
  * 控制结构中关键词如`if`后面必须有一个空格，而函数或者方法后面不能跟空格，如`funName()`；
  * 控制结构中的开始`{`与语句在同一行，而结束`}`须另起一行；
  * 控制结构中的开始圆括号后不能有空格，结束圆括号之前也不能有空格。

### Examples

```php
    
    <?php
    namespace Vendor\Package;
    
    use FooInterface;
    use BarClass as Bar;
    use OtherVendor\OtherPackage\BazClass;
    
    class Foo extends Bar implements FooInterface
    {
        public function sampleFunction($a, $b = null)
        {
            if ($a === $b) {
                bar();
            } elseif ($a > $b) {
                $foo->bar($arg1);
            } else {
                BazClass::bar($arg2, $arg3);
            }
        }
    
        final public static function bar()
        {
            // method body
        }
    }
```

* * *
    
```php
    <?php
    namespace Vendor\Package;
    
    use FooClass;
    use BarClass as Bar;
    use OtherVendor\OtherPackage\BazClass;
    
    class ClassName extends ParentClass implements \ArrayAccess, \Countable
    {
        // constants, properties, methods
    }
```

* * *
    
```php
    <?php
    namespace Vendor\Package;
    
    use FooClass;
    use BarClass as Bar;
    use OtherVendor\OtherPackage\BazClass;
    
    class ClassName extends ParentClass implements
        \ArrayAccess,
        \Countable,
        \Serializable
    {
        // constants, properties, methods
    }
```

* * *
    
```php
    <?php
    namespace Vendor\Package;
    
    class ClassName
    {
        public $foo = null;
    }
```

* * *
    
```php
    <?php
    namespace Vendor\Package;
    
    class ClassName
    {
        public function fooBarBaz($arg1, &$arg2, $arg3 = [])
        {
            // method body
        }
        // 参数较多的话可以这样：
        public function aVeryLongMethodName(
            ClassTypeHint $arg1,
            &$arg2,
            array $arg3 = []
        ) {
            // method body
        }
    }
```

* * *
    
```php
    <?php
    namespace Vendor\Package;
    
    abstract class ClassName
    {
        protected static $foo;
    
        abstract protected function zim();
    
        final public static function bar()
        {
            // method body
        }
    }
```

* * *
    
```php
    <?php
    if ($expr1) {
        // if body
    } elseif ($expr2) {
        // elseif body
    } else {
        // else body;
    }
    
    switch ($expr) {
        case 0:
            echo 'First case, with a break';
            break;
        case 1:
            echo 'Second case, which falls through';
            // no break
        case 2:
        case 3:
        case 4:
            echo 'Third case, return instead of break';
            return;
        default:
            echo 'Default case';
            break;
    }
    
    while ($expr) {
        // structure body
    }
    
    do {
        // structure body;
    } while ($expr);
    
    for ($i = 0; $i < 10; $i++) {
        // for body
    }
    
    foreach ($iterable as $key => $value) {
        // foreach body
    }
    
    try {
        // try body
    } catch (FirstExceptionType $e) {
        // catch body
    } catch (OtherExceptionType $e) {
        // catch body
    }
```

有省略。详细参考 [PSR-2](http://www.php-fig.org/psr/psr-2/)

