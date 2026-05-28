> [!NOTE]
> This adds a **Blue** bar. Great for general information.

> [!TIP]
> This adds a **Green** bar. Perfect for advice or shortcuts.

> [!IMPORTANT]
> This adds a **Purple** bar. Use this for crucial missing pieces.

> [!WARNING]
> This adds an **Amber/Yellow** bar. Good for things to watch out for.

> [!CAUTION]
> This adds a **Red** bar. Use this for things that might break your code.

<details>
<summary>📘 Click here to expand my Bash Test Notes</summary>

Inside here, you can write regular markdown:
* `[ ]` is the old standard
* `[[ ]]` is the modern way

</details>

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

Example :- 

#!/bin/bash
age=18

if [ "$age" -ge 18 ]
then
    echo "You are eligible to vote."
else
    echo "You are too young to vote."
fi

