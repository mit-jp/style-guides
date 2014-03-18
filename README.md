Style Guide for MIT JPSPGC GAMS code
====================================

This is a guide to writing code in (for now) GAMS and MPSGE. These languages are used for computable general equilibrium (CGE) economic modelling at the Joint Program.

It is much easier to understand a large codebase when all the code in it is in a consistent style ([Google][google-styleguide]).

Please feel free to add to the guide or its organization. For contributions that may raise some controversy, consider filing an [issue](https://github.com/mit-jp/style-guides/issues) first.

Code layout
-----------

### Indentation

- **Use 2 spaces per indentation level.** Do not use tabs; different editors will render tabs as 2, 4 or 8 characters wide, leading to inconsistent code.

### Maximum line length

GAMS: Limit all lines to a maximum of 79 characters.

MPSGE: No limit.

Specific statements and constructs
----------------------------------

- **Specify dimensions in declarations.** Although the following code works:

    ```
    set  s  'Example set'  / a, b, c /;
    parameter  p  'Example parameter;
    p(s) = 1;
    ```

    The following is more clear if the first reference to a parameter is in a separate file or further away in the code:

    ```
    set  s  'Example set'  / a, b, c /;
    parameter  p(s)  'Example parameter;
    p(s) = 1;
    ```

- **Do not use the universal set (`*`), unless needed for specific purpose.** In multi-dimensional parameters, a declaration using the universal set multiple times does not clearly indicate what the order of the dimensions is.

- **Pluralize.** GAMS has `set` and `sets`, `parameter` and `parameters` declarations. Use these appropriately: `set` when declaring a single set, `sets` when declaring multiple sets, etc.

Cross-platform compatibility
----------------------------

- **Use `%slash%` instead of `/` or `\`.** The forward and backward slash characters are path separators in Unix (incl. Linux and Mac OS) and Windows respectively. Hard-coding one of these, for instance in `$include` statements, guarantees that the code will *not* run on the other OS. Instead, use a variable like `%slash%`, declared *only once* as follows:

    ```
    $setglobal slash \
    $if %system.filesys% == UNIX $setglobal slash /
    ```

References
----------
### GAMS/MPSGE
- [Bruce McCarl's "Expanded GAMS User Guide"][gams-doc-mccarl]

[gams-doc-official]: http://www.gams.com/docs/document.htm
[gams-doc-contrib]: http://www.gams.com/docs/contributed/
[gams-doc-mccarl]: http://www.gams.com/mccarl/mccarlhtml/
[google-styleguide]: https://code.google.com/p/google-styleguide/

### Other style guides
- https://code.google.com/p/google-styleguide/
- http://legacy.python.org/dev/peps/pep-0008/
- https://www.kernel.org/doc/Documentation/CodingStyle
