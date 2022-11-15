# General purpose JavaScript deobfuscator

A powerful deobfuscator to remove common JavaScript obfuscation techniques.
Open an issue if there is a feature you think should be implemented.


Install via `npm install js-deobfuscator`

## Features

-   Unpacks arrays containing literals (strings, numbers etc) and replaces all references to them
-   Removes simple proxy functions (calls to another function), array proxy functions and arithmetic proxy functions (binary expressions)
-   Simplifies arithmetic expressions
-   Simplifies string concatenation
-   Renames unreadable hexadecimal identifiers (e.g. \_0xca830a)
-   Converts computed to static member expressions and beautifies the code
-   _Experimental_ function evaluation



Often obfuscated scripts don't just use an array of strings, instead they have string decoder functions that execute more complex logic, such as the example below.

A few important points about function evaluation:

-   BE CAREFUL when using function evaluation, this executes whatever functions you specify on your local machine so make sure those functions are not doing anything malicious.
-   This feature is still somewhat experimental, it's probably easier to use via the CLI as it's easier to find errors than the online version.
-   If the function is not a function declaration (i.e. a function expression or an arrow function expression) then the deobfuscator will not be able to detect the name of it automatically. To provide it use "#execute[name=FUNC_NAME]" directive.
-   You may need to modify the function to ensure it relies on no external variables (i.e. move a string array declaration inside the function) and handle any extra logic like string array rotation first.
-   You must first remove any anti tampering mechanisms before using function evaluation, otherwise it may cause an infinite loop.

## Config

```typescript
interface Config {
    arrays: {
        unpackArrays: boolean;
        removeArrays: boolean;
    };
    proxyFunctions: {
        replaceProxyFunctions: boolean;
        removeProxyFunctions: boolean;
    };
    expressions: {
        simplifyExpressions: boolean;
        removeDeadBranches: boolean;
    };
    miscellaneous: {
        beautify: boolean;
        simplifyProperties: boolean;
        renameHexIdentifiers: boolean;
    };
}
```

## To Run

Either install the module locally via `npm install js-deobfuscator` and import as usual or install globally `npm install -g js-deobfuscator` and use the `js-deobfuscator` CLI:

```shell
> js-deobfuscator -h
Usage: run [options]

Deobfuscate a javascript file

Options:
  -i, --input [input_file]    The input file to deobfuscate (default: "input/source.js")
  -o, --output [output_file]  The deobfuscated output file (default: "output/output.js")
  -f, --force                 Whether to overwrite the output file or not
  -h, --help                  display help for command

>
```
