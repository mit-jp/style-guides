Style Guide for MIT JPSPGC GAMS code
====================================

This is a guide to writing code in (for now) GAMS and MPSGE. These languages are used for computable general equilibrium (CGE) economic modelling at the Joint Program.

It is much easier to understand a large codebase when all the code in it is in a consistent style ([Google][google-styleguide]).

Please feel free to add to the guide or its organization. For contributions that may raise some controversy, consider filing an [issue](https://github.com/mit-jp/style-guides/issues) first.

Code layout
-----------

- **Use 2 spaces per indentation level.** Do not use tabs; different editors will render tabs as 2, 4 or 8 characters wide, leading to inconsistent code.
- **Limit all lines in GAMS source to a maximum of 79 characters.**
- **Where possible, limit lines in MPSGE source to a maximum of 79 characters.**
- **Use lowercase for GAMS keywords and variable names.**
- **Use uppercase for MPSGE variable names.**
- **Use 2 spaces between the keyword, name(s), explanatory text and values in declarations.** For instance:

    ```
    set  s  'Example set'  / a, b, c /;
    ```

- **Use 1 space after a comma in the list of arguments to a function.**
- **Use 0 spaces after commas in dimension indices to a set or parameter.**
- **Put each set, parameter or scalar in a multiple declaration on a separate line.** Put the closing semicolon on its own line. Align the explanatory text and values.

    ```
    parameters
      g     'Sectors'  / agr, ele, food, for /
      e(g)  'Energy'   / ele /
      ;
    ```

Comments
--------

- **Enter explanatory text for all declared variables.** If the variable has a physical interpretation (economic value, energy, emissions, etc.) include the units, in parentheses.

Directory structure
-------------------

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

- **Do not use the universal set (`*`), unless needed for a specific purpose.** In multi-dimensional parameters, a declaration using the universal set multiple times does not clearly indicate what the order of the dimensions is.

- **Pluralize.** GAMS has `set` and `sets`, `parameter` and `parameters` declarations. Use these appropriately: `set` when declaring a single set, `sets` when declaring multiple sets, etc.

Cross-platform compatibility
----------------------------

- **Use `%slash%` instead of `/` or `\`.** The forward and backward slash characters are path separators in Unix (incl. Linux and Mac OS) and Windows respectively. Hard-coding one of these, for instance in `$include` statements, guarantees that the code will *not* run on the other OS. Instead, use a variable like `%slash%`, declared *only once* as follows:

    ```
    $setglobal slash \
    $if %system.filesys% == UNIX $setglobal slash /
    ```

- **Check platform before using platform-specific utilities.** For instance, `gdxxrw` (convert GDX files to MS Excel spreadsheets) is only available on Windows. One way to do this is to check the value of a variable, e.g. `%slash%`, using `$if`.


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
