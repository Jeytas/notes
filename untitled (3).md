# 07.11.17

http://oliverelliott.org/article/computing/tut_unix/28/

Edit the [https://gist.github.com/Jeytas/c97300b44df3321d25411757ccae5399](installer.sh) and make use of the different exit statuses 
so that it can run more safely.

# 09.11.17

## Loops
### For-loops

For-Loops follow the following pattern: `for *variable* in *list*; do ...; done`

For-Loops can have several data types in one

### While-loops

Pattern: `while *condition*; do ...; done` => Structure is similar to for-loops

`x=1; while ((x <= 3)); do echo $x; ((x++)); done` => Will output "1 2 3"

## Arguments to a script

Bash scripts can take arguments directly from the command line. Suppose we have a script called "myscript.sh" and we add an argument to it like so "myscript.sh myargument" then the argument will be stored in the `$1` variable. If we add a second argument, it will be stored in `$2` and so on. Bash stores these arguments inside an array and the variables refer to the position in the array.

There is a built-in tool for handling arguments called `getopts` (similar to optparse in Ruby), it is however often a lot easier to just create your own option parser using a while-loop and if-statements.

```bash
#!/bin/bash
    
helpmessage="This script showcases how to read arguments"
    
### get arguments
# while input array size greater than zero
while (($# > 0)); do
if [ "$1" == "-h" -o "$1" == "-help" -o "$1" == "--help" ]; then
    shift; 
    echo "$helpmessage"
    exit;
elif [ "$1" == "-f1" -o "$1" == "--flag1" ]; then
    # store what's passed via flag1 in var1
    shift; var1=$1; shift
elif [ "$1" == "-f2" -o "$1" == "--flag2" ]; then
    shift; var2=$1; shift
elif [ "$1" == "-f3" -o "$1" == "--flag3" ]; then
    shift; var3=$1; shift
# if unknown argument, just shift
else    
    shift
fi
done
    
### main
# echo variable if not empty 
if [ ! -z $var1 ]; then echo "flag1 passed "$var1; fi
if [ ! -z $var2 ]; then echo "flag2 passed "$var2; fi
if [ ! -z $var3 ]; then echo "flag3 passed "$var3; fi
```

There are a few new things in this self-made option parser which are the following

 - `$#` is the size of the argument array 
 - `shift` removes an element from the array (Same in Perl)
 - `exit` exists the script (stops it)
 - `-o` is a logical or in Bash ( || in Ruby)
 - `-z` checks whether or not a variable is empty

Running the script as `./test_args --flag1 x -f2 y --flag3 zzz` would then produce the following output:

    flag1 passed x
    flag2 passed y
    flag3 passed zzz
    
However, this is where we meet the limits of Bash. Rather than using Bash to create script with arguments, it would be wiser to do so in another scripting language such as Perl.

## Here Document

A here document is used very commonly within Linux. Withing Bash scripts, it is primarily used to create strings that span multiple lines like so:

```bash
cat <<_EOF_

Usage:

$0 --flag1 STRING [--flag2 STRING] [--flag3 STRING]

Required Arguments:

  --flag1 STRING	This argument does this

Options:

  --flag2 STRING	This argument does that
  --flag3 STRING	This argument does another thing

_EOF_
```
Everything between the `_EOF_` tags is treated as a simple string and not a command.

## Command line shortcuts
Because editing in the command line can be quite cumbersome, there are shortcuts. By default, these shortcuts use Emacs-style keybindings which are as follows:
- Cntrl-a - jump cursor to beginning of line
- Cntrl-e - jump cursor to end of line
- Cntrl-k - delete to end of line
- Cntrl-u - delete to beginning of line
- Cntrl-w - delete back one word
- Cntrl-y - paste (yank) what was deleted with the above shortcuts
- Cntrl-r - reverse-search history for a given word
- Cntrl-c - kill the process running in the foreground; don't execute current line on the command line
- Cntrl-z - suspend the process running in the foreground
- Cntrl-l - clear screen. (this has an advantage over the unix command clear in that it works in the Python, MySQL, and other shells)
- Cntrl-d - end of transmission (in practice, often synonymous with quit - e.g., exiting the Python or MySQL shells)
- Cntrl-s - freeze screen
- Cntrl-q - un-freeze screen

However, Vim-style keybindings can also be enabled instead of the Emacs-style ones by typing (or adding to the .bashrc) the following command `set -o vim` and it can be reversed to Emacs-style using `set -o emacs`.



## Other

To specify a range of one data type, use the following construction: `{1..10}` or `{a..z}`

To create three folders (1-3) each with two subfolders (X and Y) you can do the following `mkdir -p myfolder{1..3}/{X,Y}`





