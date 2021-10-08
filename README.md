# CraftSmithFight
This is a simple game built by Cormac Conahan and Chris Taylor using Unity Game Engine

# **Cormac Conahan CSCI 320 Assignment 5**

## **Regular Expressions**

**Write a regular expression that will recognize each of the following:**

a) A lower-case alphabetic character followed by any number of either lower case alphabetic or numeric digits. E.g., fn234nn98

`([a-z])(([a-z])|([0-9]))*`

b) Zero or more numeric digits followed, optionally, by a decimal point and zero or more numeric digits. E.g., 123.456

To strictly give the answer required by this problem, the regular expression would yield:

`([0-9])*(\.)?([0-9])*`

However, this would match everything because every search parameter can be zero or more.

A safer answer to this problem would be:

`([0-9])+(\.)?([0-9])*`

since this makes sure at least one number is present.

## **Backus-Naur Form**

Given the following BNF:

    1. commandline ::= list
        |   list ";"
        |   list "&"

    2. list ::=  conditional
        |   list ";" conditional
        |   list "&" conditional

    3. conditional ::=  pipeline
        |conditional "&&" pipeline
        |   conditional "||" pipeline

    4. pipeline ::=  command
        |   pipeline "|"command

    5. command  ::=  word
        |   redirection
        |   command word
        |   command redirection
        
    6. redirection  ::=  redirectionop filename
   
    7. redirectionop  ::=  "<"  |  ">"  |  "2>"

Assume words are defined as in section 2.a above. Further, spaces are assumed to delimit words. Show the derivations (as in class or Figure 6.5 in the text) of the following statements in the language:

### **a) `cat myfile && cat yourfile`**

Line | Production | Result
-----|------------|-------
1 | (1) | list
2 | (2) | conditional
3 | (3) | conditional "&&" pipeline
4 | (3) | pipeline "&&" pipeline
5 | (4) | command "&&" pipeline
6 | (5) | command "myfile" "&&" pipeline
7 | (5) | "cat" "myfile" "&&" pipeline
8 | (4) | "cat" "myfile" "&&" command
9 | (5) | "cat" "myfile" "&&" command "yourfile"
10 | (5) | "cat" "myfile" "&&" "cat" "yourfile"

### **b) `ls | grep something > somefile`**

Line | Production | Result
-----|------------|-------
1 | (1) | list
2 | (2) | conditional
3 | (3) | pipeline
4 | (4) | pipeline "\|" command
5 | (4) | command "\|" command
6 | (5) | "ls" "\|" command
7 | (5) | "ls" "\|" command redirection
8 | (5) | "ls" "\|" command "something" redirection
9 | (5) | "ls" "\|" "grep" "something" redirection
10 | (6) | "ls" "\|" "grep" "something" redirectionop "somefile"
11 | (7) | "ls" "\|" "grep" "something" ">" "somefile"

### **c) `somecmd someparm || othercmd; gcc myfile > output`**

Line | Production | Result
-----|------------|-------
1 | (1) | list
2 | (2) | list ";" conditional
3 | (2) | conditional ";" conditional
4 | (3) | conditional "\|\|" pipeline ";" conditional
5 | (3) | pipeline "\|\|" pipeline ";" conditional
6 | (4) | command "\|\|" pipeline ";" conditional
7 | (5) | command "someparm" "\|\|" pipeline ";" conditional
8 | (5) | "somecmd" "someparm" "\|\|" pipeline ";" conditional
9 | (4) | "somecmd" "someparm" "\|\|" command ";" conditional
10 | (5) | "somecmd" "someparm" "\|\|" "othercmd" ";" conditional
11 | (3) | "somecmd" "someparm" "\|\|" "othercmd" ";" pipeline
12 | (4) | "somecmd" "someparm" "\|\|" "othercmd" ";" command
13 | (5) | "somecmd" "someparm" "\|\|" "othercmd" ";" command redirection
14 | (5) | "somecmd" "someparm" "\|\|" "othercmd" ";" command "myfile" redirection
15 | (5) | "somecmd" "someparm" "\|\|" "othercmd" ";" "gcc" "myfile" redirection
16 | (6) | "somecmd" "someparm" "\|\|" "othercmd" ";" "gcc" "myfile" redirectionop "output"
17 | (7) | "somecmd" "someparm" "\|\|" "othercmd" ";" "gcc" "myfile" ">" "output"

### **d) `cmd1; cmd2 && cmd3; less output`**

Line | Production | Result
-----|------------|-------
1 | (1) | list
2 | (2) | list ";" conditional
3 | (2) | list ";" conditional ";" conditional
4 | (2) | conditional ";" conditional ";" conditional
5 | (3) | pipeline ";" conditional ";" conditional
6 | (4) | command ";" conditional ";" conditional
7 | (5) | "cmd1" ";" conditional ";" conditional
8 | (3) | "cmd1" ";" conditional "&&" pipeline ";" conditional
9 | (3) | "cmd1" ";" pipeline "&&" pipeline ";" conditional
10 | (4) | "cmd1" ";" command "&&" pipeline ";" conditional
11 | (5) | "cmd1" ";" "cmd2" "&&" pipeline ";" conditional
12 | (4) | "cmd1" ";" "cmd2" "&&" command ";" conditional
13 | (5) | "cmd1" ";" "cmd2" "&&" "cmd3" ";" conditional
14 | (3) | "cmd1" ";" "cmd2" "&&" "cmd3" ";" pipeline
15 | (4) | "cmd1" ";" "cmd2" "&&" "cmd3" ";" command
16 | (5) | "cmd1" ";" "cmd2" "&&" "cmd3" ";" command "output"
17 | (5) | "cmd1" ";" "cmd2" "&&" "cmd3" ";" "less" "output"
