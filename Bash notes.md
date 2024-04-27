### Selecting an Interpreter
You can set the interpreter for your shell script to run on by including it in your command
- `echo $SHELL` in terminal shows your current interpreter

_syntax:_

`#!/path/to/interpreter` i.e `#!/bin/bash` or `#!/bin/sh`

After this, you can simply run your script with `./script.sh` rather than `bash script.sh` when the interpreter has not been set

_Note:_ You have to make the file executable using `chmod +x script.sh` 

### Variables

It is a best practice to store file locations as variables rather than raw path when executing file commands.

_Example:_ Bash script to copy file from one location to another

```bash

 FROM_LOCATION=/home/user/documents
 TO_LOCATION=/home/user/downloads

 cp $FROM_LOCATION $TO_LOCATION #Path stored as a variable
 cp "$FROM_LOCATION/filetocopy" "$TO_LOCATION/copiedfile"
```

- **read** statement is used to get input from a user

_Example:_ Bash script to greet a new user

```bash

 echo What is your First Name?
 read FIRST_NAME #Storing the user input into the variable FIRST_NAME
 echo What is your Last Name?
 read LAST_NAME

 echo "Hello $FIRST_NAME, $LAST_NAME; Welcome to Sensei Corp"
```
### Positional Arguments

Positional arguments are arguments placed in a certain position behind the command or script to be run. The arguments are separated by a space and positions starts from 1 reserving 0 for the shell command itself.

_Syntax:_ `./add.sh Bill Gates`

Bill is position 1 argument and Gates is position 2 argument

_Example:_ Script to combine inputs in position 1 and 2

```bash
 echo "Hello Mr. $1 $2"
 # Variables $1 and $2 represents arguments in position 1 and 2 
```

- **Piping**: This is a way of sending the output of one command as input of another command, it is represented as `|` i.e `ls -l /root | grep home`
- **Grep**: This command is used to filter for specific words. As seen in te previous example, it filters for "home" in the directory listing.

### Output Redirection

It is a way of sending output of a command to a file

- Greater than (>): It is a symbol used to write to a file
- Double greater than (>>): It is a symbol used to append to a file
- Less than (<): It is used to feed data to a command

_Example:_ 

```bash
 wc -w < hello.txt #This returns the word count of words in the file hello.txt
```

- Double less than (<<): It is used to enter data in the command line in an interactive manner and also outputs it

_Example:_

```bash
cat << EOF 
#Data to display
#New line with enter
EOF #terminates the command and displays all that has been entered

```

### Test Operators

This is used to verify if a couple of arguments is true or not, it is done by using an in-built command `test` or `[ ]`

_Example:_

```bash

 [ hello = hello ] #checks if the first string is equal to the second string
 echo $? # $? is used to return the exit code of the last executed command which in this case will be 0 because the test is true
```

- `-eq` is also a test operator can also be used in place of an equal sign

### Conditional Statements  (If/Elif/Else)

They are used in conjuction with test operators to validate certain conditions in complex codes.

_Syntax:_

```bash
 if [ condition ]; then
    #code to execute
elif [ condition ]; then
    #Another code to execute
else
    #final code to execute
fi 

#fi closes the if code block
```

_Example:_ Script to check logged in user

```bash
 if [ ${1,,} = mark ]; then
    echo "Welcome boss $1"
 elif [ ${1,,} = help ]; then
    echo "Just enter your username, human!"
 else
    echo "I don't know who you are nor are you my boss"
 fi
```
_NB:_ The double comma `,,` and curly braces `{}` is known as **parameter expansion** and allows to ignore upper and lower cases when comparing values

### Case Statements

A case statement performs different actions depending on which case is true.

This is similar to if/elif/else statements but it is easier to read and better when checking for multiple values.

_Syntax:_

```bash
 case ${1,,} in
    option 1 | option 2)
        #Code to execute
        ;; # closes the option
    option 3)
        #Code to execute
        ;;
    *) # Default option to catch any other input different from the specified choices 
        #Code to execute
        ;;
 esac 
 
 #esac closes the case block
```

_NB:_ `|` is a separator for multiple options in this case and `)` is always required after setting an option

### Arrays

It is a way of assigning multiple values to a variable, collected in a list. An array in bash is seperated with a space.

_Syntax:_ `ARRAY_OF_NUMBERS=(one two three four five)`

**Calling Elements in the array**

- `echo ${ARRAY_OF_NUMBERS[@]}` Prints all elements in the array
- `echo ${ARRAY_OF_NUMBERS[i]}` Prints an element in a certain position, i = index (0,1,2...)

### For Loops

It can used to run through the items in an array.

_Syntax:_

```bash
for item in ${ARRAY_LIST[@]}; do
    #code to execute
done
```

_Example:_ Bash script to count letters of elements in an array

```bash
CAR_MAKERS=(Honda Toyota RAM Tata)

for maker in ${CAR_MAKERS[@]}; do
    echo -n $maker | wc -c # -n ignores new line characters and -c counts each letter(bytes)
done

```

### Functions

They are little programs within a script that can also be run within the script. It makes our code reusable. A function is called in bash just by typing its name i.e `showtime` 

_Syntax:_

```bash
funtionname()
{
    #codeblock
}
```

_Example:_ Function to display how long a machine has been running for

```bash

showuptime()
{
    up=$(uptime -p)
    since=$(uptime -s)

cat << EOF
 -----------------------------------------
 This machine has been up for ${up} 
 And has been running since ${since}
 -----------------------------------------
EOF
}
# {} are important because we're using cat command, wouldn't be required if it was echo command
```

_NB:_ 

- When assigning the output of a command to variable, it must be put in brackets with a dollar sign i.e `workingdir=$(pwd)`
- When within a function, it is important to define local variables so that it doesn't conflict or override global variables outside of the function with the same name. i.e `local up=$(uptime -p)`
- Positional arguments can also be passed into a function


### Exit codes

- Exit code 0: Program ran successfully or if condition is true
- Exit code 1: Program encountered an error or if condition is false

_Usage:_

```bash

showname()
{
    if [ ${1,,} = mark ]; then
        echo "Hello $1"
        return 0
    else
        return 1
    fi
}

showname $1
if [ $? = 1 ]; then #If the exit code is 1, the program should print the username and an error message
    echo "Unrecognized user"
fi

```

### Other Bash Commands

**AWK**

It is used to filter file contents or the output of a command in such a way that we can print out the most essential parts. This is done by piping the output of a command into `awk`

_Usage_

- `awk '{print $1}' testfile.txt`: This prints the first index of the testfile.txt and it can be changed as we want i.e ($2, $3). It uses space as a seperator
- `awk -F, '{print $1}' testfile.csv`: This specifies a separator using the `-F` flag, in this case a comma `,`
- `echo "Just enter a value: username" | awk -F: '{print $1}'`: This pipes output of the echo command into awk and using `:` as a separator prints the second value which in this case is `username`.

_NB:_ If we want to remove the preceding space, it is piped again into `cut -c2-` (`2-` prints the second character and everything after it)

**SED**

It is a bash command that is used to replace certain values in text files using regular expressions.

_Usage_

```bash
sed 'mode/[word to substitute]/[replacement]/[how the change should be effected]' [filename]
```

_Example:_ replace every occurence of fly with grasshopper in a text file

```bash
sed 's/fly/grasshopper/g' animals.txt
# s - represents subtitute mode
# g - reperesents globally, which means anywhere there is fly in the text file will be replaced with grasshopper
```
- To make a backup of the original file before making changes: `-i.ORIGINAL` flag is added after the sed command 