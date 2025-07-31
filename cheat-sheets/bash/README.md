# Bash

* [http://mywiki.wooledge.org/BashGuide](http://mywiki.wooledge.org/BashGuide)
* [https://devhints.io/bash](https://devhints.io/bash)
* [https://learnxinyminutes.com/docs/bash/](https://learnxinyminutes.com/docs/bash/)

## Spezielle Charaktere

<table data-header-hidden><thead><tr><th width="98"></th><th></th></tr></thead><tbody><tr><td><strong>Char.</strong></td><td><strong>Description</strong></td></tr><tr><td>" "</td><td><em>Whitespace</em> — this is a tab, newline, vertical tab, form feed, carriage return, or space. Bash uses whitespace to determine where words begin and end. The first word is the command name and additional words become arguments to that command.</td></tr><tr><td>$</td><td><em>Expansion</em> — introduces various types of expansion: parameter expansion (e.g. $var or ${var}), <a href="http://mywiki.wooledge.org/CommandSubstitution">command substitution</a> (e.g. $(command)), or arithmetic expansion (e.g. $((expression))). More on expansions later.</td></tr><tr><td>''</td><td><em>Single quotes</em> — protect the text inside them so that it has a <em>literal</em> meaning. With them, generally any kind of interpretation by Bash is ignored: special characters are passed over and multiple words are prevented from being split.</td></tr><tr><td>""</td><td><em>Double quotes</em> — protect the text inside them from being split into multiple words or arguments, yet allow substitutions to occur; the meaning of most other special characters is usually prevented.</td></tr><tr><td>\</td><td><em>Escape</em> — (backslash) prevents the next character from being interpreted as a special character. This works outside of quoting, inside double quotes, and generally ignored in single quotes.</td></tr><tr><td>#</td><td><em>Comment</em> — the # character begins a commentary that extends to the end of the line. Comments are notes of explanation and are not processed by the shell.</td></tr><tr><td>=</td><td><em>Assignment</em> -- assign a value to a variable (e.g. logdir=/var/log/myprog). Whitespace is <em>not</em> allowed on either side of the = character.</td></tr><tr><td>[[ ]]</td><td><em>Test</em> — an evaluation of a conditional expression to determine whether it is "true" or "false". Tests are used in Bash to compare strings, check the existence of a file, etc. More of this will be covered later.</td></tr><tr><td>!</td><td><em>Negate</em> — used to negate or reverse a test or exit status. For example: ! grep text file; exit $?.</td></tr><tr><td>>, >>, &#x3C;</td><td><em>Redirection</em> — redirect a command's <em>output</em> or <em>input</em> to a file.</td></tr><tr><td>|</td><td><em>Pipe</em> — send the output from one command to the input of another command. This is a method of chaining commands together. Example: echo "Hello beautiful." | grep -o beautiful.</td></tr><tr><td>;</td><td><em>Command separator</em> — used to separate multiple commands that are on the same line.</td></tr><tr><td>{ }</td><td><em>Inline group</em> — commands inside the curly braces are treated as if they were one command. It is convenient to use these when Bash syntax requires only one command and a function doesn't feel warranted.</td></tr><tr><td>( )</td><td><em>Subshell group</em> — similar to the above but where commands within are executed in a <a href="http://mywiki.wooledge.org/SubShell">subshell</a> (a new process). Used much like a sandbox, if a command causes side effects (like changing variables), it will have no effect on the current shell.</td></tr><tr><td>(( ))</td><td><em>Arithmetic expression</em> — with an <a href="http://mywiki.wooledge.org/ArithmeticExpression">arithmetic expression</a>, characters such as +, -, *, and / are mathematical operators used for calculations. They can be used for variable assignments like (( a = 1 + 4 )) as well as tests like if (( a &#x3C; b )). More on this later.</td></tr><tr><td>$(( ))</td><td><em>Arithmetic expansion</em> — Comparable to the above, but the expression is replaced with the result of its arithmetic evaluation. Example: echo "The average is $(( (a+b)/2 ))".</td></tr><tr><td>*, ?</td><td><a href="http://mywiki.wooledge.org/glob"><em>Globs</em></a> -- "wildcard" characters which match parts of filenames (e.g. ls *.txt).</td></tr><tr><td>~</td><td><em>Home directory</em> — the tilde is a representation of a home directory. When alone or followed by a /, it means the current user's home directory; otherwise, a username must be specified (e.g. ls ~/Documents; cp ~john/.bashrc .).</td></tr><tr><td>&#x26;</td><td><em>Background</em> -- when used at the end of a command, run the command in the background (do not wait for it to complete).</td></tr></tbody></table>

## Parameter

<table data-header-hidden><thead><tr><th width="133.33333333333334"></th><th width="94"></th><th></th></tr></thead><tbody><tr><td><strong>Parameter Name</strong></td><td><strong>Usage</strong></td><td><strong>Beschreibung</strong></td></tr><tr><td><strong>0</strong></td><td>"$0"</td><td>Contains the name, or the path, of the script. This is not always reliable.</td></tr><tr><td><strong>1</strong> <strong>2</strong> etc.</td><td>"$1" etc.</td><td><em>Positional Parameters</em> contain the arguments that were passed to the current script or function.</td></tr><tr><td><strong>*</strong></td><td>"$*"</td><td>Expands to all the words of all the positional parameters. Double quoted, it expands to a single string containing them all, separated by the first character of the <strong>IFS</strong> variable (discussed later).</td></tr><tr><td><strong>@</strong></td><td>"$@"</td><td>Expands to all the words of all the positional parameters. Double quoted, it expands to a list of them all as individual words.</td></tr><tr><td><strong>#</strong></td><td>$#</td><td>Expands to the number of positional parameters that are currently set.</td></tr><tr><td><strong>?</strong></td><td>$?</td><td>Expands to the exit code of the most recently completed foreground command.</td></tr><tr><td><strong>$</strong></td><td>$$</td><td>Expands to the <a href="http://mywiki.wooledge.org/ProcessManagement">PID</a> (process ID number) of the current shell.</td></tr><tr><td><strong>!</strong></td><td>$!</td><td>Expands to the PID of the command most recently executed in the background.</td></tr><tr><td><strong>_</strong></td><td>"$_"</td><td>Expands to the last argument of the last command that was executed.</td></tr></tbody></table>



## Funktionen

```sh
#Ersetze Zeichen
"Text:ABC" | tr ";" "\"
Text
ABC
user@machine$ _
```









