# The Golden Rule of Bash if Statements: Spaces Matter!


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
