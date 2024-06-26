### Comparison Operators

Comparison operators are often used with conditional statements like if statement

Examples of operators:
- greater than >
- less than <
- greater than or equals to >=
- less than or equals to <=
- Equals to ==: value A is equals to value B
- Not equals to !=: Evaluate whether two objects are different
 
### Loops (Iterative statments)

- **for Loops**: they repeat codes for a specified sequence

_syntax:_

```
 for i in [1,2,3,4]:
    print(i)
```
`range()` function generates a sequence of numbers by supplying a minimum and maximum value; if no minimum value is given, it automatically assumes 0 as starting point. it can be used with for loop.

- **while loops**: executes a code repeatedly based on a condition, it continues as long as a condition is true.

_syntax:_

```
 time = 0
 while time <= 10: //specifies the condition for the loop to run
    print(time)
    time = time + 2
```

unlike for loops, variables in while loops are set outside of the loop header

while loop can also be used with break and continue to iterate through a condition until it is true then break out of it.

_syntax:_

```
 while True:
    passwd = input("Enter a password: ")
    if len(passwd) < 8:
      print("- Password too short")
      continue
    else:
      break

 //the program keeps looping until the user enters a password that is 8 or more in length before the break statement executes

```
					
### Functions

Functions are section of codes that can be reused in a program, it improves program efficiency.
There are built-in function in python e.g `print()`, `type()` etc.

`def` is used to start a function

_syntax:_ 

```
 def greet_employee(): //parameter to use are included in the ()
    print("Welcome, You are logged in!")
```

- A _parameter_ is an object that is included in a function definition for use in that function.
- An _argument_ is the data brought into a function when it is called.

`return` is used to return information from a function

**Built in Functions**
- `max()` function outputs the largest number in an input
- `sorted()` function sorts the components of a list
- `str()` function converts the input object into string
- `len()` function returns the number of elements in an object
- `exit()` function is used to exit a code at specific line to control a program flow
- `int()` function allows the conversion of an input or a string variable to an integer
- `str()` function allows the conversion of an input or integer variable to a string
- `type()` is a function used to return the data type of its input

_A type error is an error that results from using the wrong data type_

_A string variable can also be reassigned to an intentional variable_.

""" """ is used to print multiple lines of strings, same function as using \n to print on a new line.

### Methods

Methods are functions that belongs to specific data type
common string methods are `.upper()`, `.lower()` and `.title()`
- `.index()` method finds the first occurence of the input in a string and returns its location i.e `print("Hello".index("e"))` returns a value of 1.
- `.insert(position,element)` method adds an element in a specific position inside a list
- `.remove()` method removes the first occurence of an element in a list
- `.append()` adds input to the end of a list, takes only one argument
- `.extend()` also adds to the end of a list but can take multiple arguments in form of another list i.e `list.extend([])`
- `.clear()` deletes all entries in a list
- `.pop()` method allows to delete files with their index rather than string

**string slicing** is a way of extracting from a string using their indices i.e `"hello"[0:3]` outputs the first 3 index of the string "hel"

### Lists

Lists allows a variable to store multiple pieces and data of different types (i.e int, string, boolean)

_syntax:_ `my_list = ["a", "b", "c"]`
  
lists are accessed using their index value like in strings i.e `my_list[1]`.

Reverse order notation of a string - `[::-1]`

NB: _strings are immutable (can't be changed) but lists aren't_

### Regular expressions
- `+` is a regular expression that represents one or more occurences of a specific character i.e `"a+"`.
- `\w` matches with any alphanumeric character but doesn't match symbols.
combining `"\w+"` can be used together for more extended search
regular expressions can be used when the re module is imported into python

- `re.findall()` returns a list of matches to a regular expression
 
_syntax:_ 

```
 re.findall("\w+@\w+\.\w+", email_log) 
 //this outputs all emails contained in the email log file, putting \ before the dot allows python to parse it as a string and not an operator
```

- `re.search()` allows to search through a string for certain patterns and return them

_syntax:_ `re.search("[pattern]", string)`

- `re.compile()` allows to make a character set or define a pattern to pass as an argument

_syntax:_ `regex = re.compile("pattern")`

### Python for Automation (Security)

- `with` command handles errors and manages external resources, it is often used in file handling to automatically close a file after reading it.
- `open()` is a function that opens a file.
- `.read()` method converts files to strings.

_syntax:_ 

```
 with open("filepath/filename.extension","r/w/a") as file:
    file_text = file.read() or file.write("new data")
 print(file_text)
 file.close() // closes the instance of the file in the current program 
 //r indicates read, a indicates append to an existing file and w indicates write to a new file; specifies what we want to do with the file
```

- `.split()` method converts a string into a list.

using the previous example: `print(file_text.split())`

### Logical Operators

They allow for additional control in the program flow
- `AND` - checks if two or more conditions are valid for it to be true
- `OR` - checks if either of given conditions are valid for it to be true
- `NOT` - checks if a condition is invalid for it to be true


### Modules and Libraries

- Rich: this modules allows to customize texts and make interactions more beautiful
- Colorama: allows use of foreground, background and styles for command line texts
- Emoji: used for inserting emojis
- OS: Used to execute operating system commands; can be quite handy when writing exploits in python

To perform OS relates tasks via python in command line - `import os;` is used

`import os; print(os.popen("ls -l").read())` - this allows to interact with the linux shell parsing the `ls -l` command

`os.system(f"nmap -A -p- {ip}")` - Allows to run nmap command for recon on specified IP

### Hashing in Python

Hashes can be used in python by importing the hashlib library

`hash = hashlib.sha256` or `hash = hashlib.new("sha256")` - initiates a new hash instance, other hashes can also be specified. To list supported hashes in all OS `print(hashlib algorithms_guaranteed)`

- `hash.update()` - updates text/file with a specified hash
- `hash.hexdigest()` - prints a specified hash digest in hex form
- `hash.name` - prints the literal name of an hash algorithm to the terminal

_Note:_ When opening files to hash, using `"rb"` as opening mode instead of `"r"` allows the file to be opened as binary without escaping any character

If taking raw user input, make sure to encode before passing to hashlib i.e `userinput.encode()`

### Encryption & Decryption in Python

1. Symmetric-key cryptography: This involves using the same key for encryption and decryption, to do this `Fernet` module in the `cryptography` library is used.

**Steps**
- Install the cryptography library if not already present `pip install cryptography`
- Import the Fernet module `from cryptography.fernet import Fernet`
- Generate a key for encryption and decryption `key = Fernet.generate_key()`
- Start an instance of the fernet class with the generated key `fernet = Fernet(key)`
- Encrypt the message and assign its value (the text must be encoded) `encryted_msg = fernet.encrypt(message.encode())`
- Decrypt the encrypted message and decode its content before printing its output `decrypted_msg = fernet.decrypt(encrypted_msg).decode()`

2. Asymmetric-key cryptography: This involves using different set of keys for encryption (public key) and decryption (private key); to do this `rsa` library is used.

**Steps**
- Install the rsa library if not already present `pip install rsa`
- Generate public and private keys with `rsa.newkeys` method specifying key length as its parameter, minimum key length being 16 `pubKey, privKey = rsa.newkeys(1028)`
- Encrypt using the public key generated with `rsa.encrypt` method (ensure the message is encoded) `encrypted_msg = rsa.encrypt(message.encode(), pubKey)`
- Decrypt using the private key generated with `rsa.decrypt` method and decode before printig final output `rsa.decrypt(encrypted_msg, privKey).decode()`

### Command Line Arguments - using `sys.argv`

Command line arguments are a powerful way to pass parameters to a program when it is executed.

The first item in the list is always the name of the script itself, and subsequent items are the arguments passed to the script.

It is called using the `sys` library i.e `import sys`

Individual Arguments can be accessed by indexing the `sys.argv` list

- `sys.argv[0]` contains the name of the program/script
- `sys.argv[1:]` contains all other arguments on the list

**Using argparse**

Argparse module provides more options than the `sys.argv`. It has a default optional argument -h with its long version --help

_Syntax:_

```
 import argparse

 //Initialize parser
 parser = argparse.ArgumentParser(prog="Program name", description="what the program does", epilog="text at the bottom of help")

 //Adding Positional arguments
 parser.add_argument("filename", metavar="alternate display name for the argument in help", type="arg file type e.g int, str", choices=['a restricted list to choose from for the arg'])

 //Adding optional arguments
 parser.add_argument("-o", "--Output", help ="Show Output")

 //Read arguments from command line
 args = parser.parse_args()

 if args.output: //referencing the output option created earlier
    print(f"Displaying output as: {args.output}")
```
_Note:_ 

- Positional arguments must follow a particular order (as they were created)
- Optional arguments are the one's you can choose to include using `-` or `--` switch
