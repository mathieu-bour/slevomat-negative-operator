# NegationOperatorSpacing false positive

Issue: https://github.com/slevomat/coding-standard/issues/951

`SlevomatCodingStandard.Operators.NegationOperatorSpacing` returns a false positive when
assigning a negative number like `$a = -1` with property `spacesCount` set to zero.

Configuration (see [phpcs.xml](phpcs.xml):

```xml
<rule ref="SlevomatCodingStandard.Operators.NegationOperatorSpacing">
    <properties>
        <property name="spacesCount" value="0"/>
    </properties>
</rule>
```

The output of `$ ./vendor/bin/phpcs` is the following:

```
FILE: ./slevomat-negative-operator/test.php
----------------------------------------------------------------------
FOUND 1 ERROR AFFECTING 1 LINE
----------------------------------------------------------------------
 2 | ERROR | [x] Expected exactly 0 space after "-", 0 found.
----------------------------------------------------------------------
PHPCBF CAN FIX THE 1 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------

Time: 18ms; Memory: 4MB
```

Which seems very strange.

Moreover, the output of `$ ./vendor/bin/phpcbf` is the following:

```
PHP Fatal error:  Uncaught PHP_CodeSniffer\Exceptions\RuntimeException: Undefined offset: 6 in ./slevomat-negative-operator/vendor/slevomat/coding-standard/SlevomatCodingStandard/Sniffs/Operators/NegationOperatorSpacingSniff.php on line 70 in ./slevomat-negative-operator/vendor/squizlabs/php_codesniffer/src/Runner.php:606
Stack trace:
#0 ./slevomat-negative-operator/vendor/slevomat/coding-standard/SlevomatCodingStandard/Sniffs/Operators/NegationOperatorSpacingSniff.php(70): PHP_CodeSniffer\Runner->handleErrors()
#1 ./slevomat-negative-operator/vendor/squizlabs/php_codesniffer/src/Files/File.php(496): SlevomatCodingStandard\Sniffs\Operators\NegationOperatorSpacingSniff->process()
#2 ./slevomat-negative-operator/vendor/squizlabs/php_codesniffer/src/Files/LocalFile.php(91): PHP_CodeSniffer\Files\File->process()
#3 ./slevomat-negative-operator/vendor/squizlabs/php_codesniffer/src/Fixer.php(174): PHP_CodeSni in ./slevomat-negative-operator/vendor/squizlabs/php_codesniffer/src/Runner.php on line 606
```

Thus, it may be directly related to PHP_CodeSniffer itself.
