## The Golden Rule of Bash if Statements: Spaces Matter!


Bash, [ is not just a symbol; it is an actual command (an alias for the test command). Because it is a command, you must put a space after it, and a space before the closing bracket ].

```
 if [ condition ]
 then
    # code to run if condition is true
 elif [ another_condition ]
 then
    # code to run if the first condition is false and this one is true
 else
    # code to run if all conditions are false
 fi
``` 
## Numeric Comparisons (Integers Only)
Bash uses letter-based flags for numbers, not standard symbols like <, >, or =. Think of them as abbreviations.

```
Flag,Meaning,Equivalent
-eq,Equal to,==
-ne,Not equal to,!=
-gt,Greater than,>
-ge,Greater than or equal to,>=
-lt,Less than,<
-le,Less than or equal to,<=
```