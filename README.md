Tests "README.MD".

```php
    <?php

    define(CONST_BY_DEFINE, false);

    // #8: op = DECLARE_CONST
    // #8: operands = 'NOT_FIXED_AT_COMPILE', <unknown>
    const NOT_FIXED_AT_COMPILE = CONST_BY_DEFINE; // "CONST_BY_DEFINE" is "<unknown>".

    // #10: op = DECLARE_CONST
    // #10: operands = 'FIXED_AT_COMPILE', false
    const FIXED_AT_COMPILE = false; // "false" is not "<unknown>".

    if (NOT_FIXED_AT_COMPILE) {
        echo 'Debug code which cannot delete.';
    }

    // #17: op = FETCH_CONSTANT
    // #17: return = ~0
    // #17: operands = 'FIXED_AT_COMPILE'

    // #18: op = JMPZ
    // #18: operands = ~0, ->21
    if (FIXED_AT_COMPILE) { // We can delete from this line.
        echo 'Debug code which can delete.';
    } // We can delete until this line.

    /* Result of shortcut command "%SystemRoot%\system32\cmd.exe /k php -dopcache.enable_cli=1 -dvld.active=1 -dvld.execute=0 -dvld.verbosity=0 -dvld.dump_paths=0 C:/xampp/htdocs/Pear-Package/OPcacheTestA.php".
    line     # *  op                           fetch          ext  return  operands
    ---------------------------------------------------------------------------------
    3     0  >   EXT_STMT
            1      EXT_FCALL_BEGIN
            2      FETCH_CONSTANT                                   ~0      'CONST_BY_DEFINE'
            3      SEND_VAL                                                 ~0
            4      SEND_VAL                                                 false
            5      DO_FCALL                                      2          'define'
            6      EXT_FCALL_END
    7     7      EXT_STMT
            8      DECLARE_CONST                                            'NOT_FIXED_AT_COMPILE', <unknown>
    11     9      EXT_STMT
            10      DECLARE_CONST                                            'FIXED_AT_COMPILE', false
    13    11      EXT_STMT
            12      FETCH_CONSTANT                                   ~0      'NOT_FIXED_AT_COMPILE'
            13    > JMPZ                                                     ~0, ->16
    14    14  >   EXT_STMT
            15      ECHO                                                     'Debug+code+which+cannot+delete.'
    23    16  >   EXT_STMT
            17      FETCH_CONSTANT                                   ~0      'FIXED_AT_COMPILE'
            18    > JMPZ                                                     ~0, ->21
    24    19  >   EXT_STMT
            20      ECHO                                                     'Debug+code+which+can+delete.'
    58    21  >   EXT_STMT
            22    > RETURN                                                   1
    */

    // We can delete "#16 - #20".

    ?>
```
