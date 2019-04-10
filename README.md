Silverpeas's UI Compressor - The Silverpeas JavaScript and CSS Compressor
=========================================================

The Silverpeas's UI Compressor is a fork of YUI Compressor which is a JavaScript compressor which, in addition to removing
comments and white-spaces, obfuscates local variables using the smallest
possible variable name. This obfuscation is safe, even when using constructs
such as 'eval' or 'with' (although the compression is not optimal in those
cases).

It is also able to safely compress CSS files. The decision
on which compressor is being used is made on the file extension (js or css)

Building
--------

    mvn clean install

Testing
-------

    ./tests/suite.sh


Global Options
--------------

    -h, --help
        Prints help on how to use the Silverpeas's UI Compressor

    --line-break
        Some source control tools don't like files containing lines longer than,
        say 8000 characters. The linebreak option is used in that case to split
        long lines after a specific column. It can also be used to make the code
        more readable, easier to debug (especially with the MS Script Debugger)
        Specify 0 to get a line break after each semi-colon in JavaScript, and
        after each rule in CSS.

    --type js|css
        The type of compressor (JavaScript or CSS) is chosen based on the
        extension of the input file name (.js or .css) This option is required
        if no input file has been specified. Otherwise, this option is only
        required if the input file extension is neither 'js' nor 'css'.

    --charset character-set
        If a supported character set is specified, the Silverpeas's UI Compressor will use it
        to read the input file. Otherwise, it will assume that the platform's
        default character set is being used. The output file is encoded using
        the same character set.

    -o outfile

        Place output in file outfile. If not specified, the Silverpeas's UI Compressor will
        default to the standard output, which you can redirect to a file.
        Supports a filter syntax for expressing the output pattern when there are
        multiple input files.  ex:
            java -jar silverpeas-ui-compressor.jar -o '.css$:-min.css' *.css
        ... will minify all .css files and save them as -min.css

    -v, --verbose
        Display informational messages and warnings.

JavaScript Only Options
-----------------------

    --nomunge
        Minify only. Do not obfuscate local symbols.

    --preserve-semi
        Preserve unnecessary semicolons (such as right before a '}') This option
        is useful when compressed code has to be run through JSLint

    --disable-optimizations
        Disable all the built-in micro optimizations.

Notes
-----

* If no input file is specified, it defaults to stdin.

* Supports wildcards for specifying multiple input files.

* The Silverpeas's UI Compressor requires Java version >= 1.8.

* It is possible to prevent a local variable, nested function or function
argument from being obfuscated by using "hints". A hint is a string that
is located at the very beginning of a function body like so:
    
```
function fn (arg1, arg2, arg3) {
    "arg2:nomunge, localVar:nomunge, nestedFn:nomunge";

    ...
    var localVar;
    ...

    function nestedFn () {
        ....
    }

    ...
}
```
The hint itself disappears from the compressed file.

* C-style comments starting with `/*!` are preserved. This is useful with
    comments containing copyright/license information. As of 2.4.8, the '!'
    is no longer dropped by Silverpeas's UI Compressor. For example:

```
/*!
 * TERMS OF USE - EASING EQUATIONS
 * Open source under the BSD License.
 * Copyright 2001 Robert Penner All rights reserved.
 */
```

remains in the output, untouched by Silverpeas's UI Compressor.

Modified Rhino Files
--------------------

Silverpeas's UI  Compressor uses a modified version of the Rhino library
(http://www.mozilla.org/rhino/) The changes were made to support
JScript conditional comments, preserved comments, unescaped slash
characters in regular expressions, and to allow for the optimization
of escaped quotes in string literals.

Copyright And License
---------------------

Copyright (c) 2013 Yahoo! Inc.  All rights reserved.
The copyrights embodied in the content of this file are licensed
by Yahoo! Inc. under the BSD (revised) open source license.
