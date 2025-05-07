# Advanced Elvish Shell Tutorial

## Table of Contents

- [Introduction to Elvish](#introduction-to-elvish)
- [Part 1: Getting Started with Elvish](#part-1-getting-started-with-elvish)
  - [Installing Elvish](#installing-elvish)
  - [Your First Commands](#your-first-commands)
- [Part 2: Variables and Data Types](#part-2-variables-and-data-types)
  - [Working with Variables](#working-with-variables)
  - [Lists](#lists)
  - [Maps](#maps)
- [Part 3: Control Structures](#part-3-control-structures)
  - [Conditional Statements](#conditional-statements)
  - [Loops](#loops)
- [Part 4: Pipelines and IO](#part-4-pipelines-and-io)
  - [Traditional Byte Pipelines](#traditional-byte-pipelines)
  - [Value Pipelines](#value-pipelines)
- [Part 5: Function Definitions](#part-5-function-definitions)
- [Part 6: Working with Files and Directories](#part-6-working-with-files-and-directories)
  - [Navigating Directories](#navigating-directories)
  - [Directory Navigation Shortcuts](#directory-navigation-shortcuts)
  - [File Operations](#file-operations)
- [Part 7: Advanced Value Types and Data Manipulation](#part-7-advanced-value-types-and-data-manipulation)
  - [Typed Numbers vs String Numbers](#typed-numbers-vs-string-numbers)
  - [Rational and Floating-Point Numbers](#rational-and-floating-point-numbers)
  - [Complex Data Structure Operations](#complex-data-structure-operations)
- [Part 8: Modules and Code Organization](#part-8-modules-and-code-organization)
  - [Exploring Built-in Modules](#exploring-built-in-modules)
  - [Creating Your Own Module](#creating-your-own-module)
  - [Module Search Paths](#module-search-paths)
- [Part 9: Function Programming](#part-9-function-programming)
  - [Anonymous Functions (Lambdas)](#anonymous-functions-lambdas)
  - [Closures](#closures)
  - [Higher-Order Functions](#higher-order-functions)
- [Part 10: Exception Handling](#part-10-exception-handling)
  - [Exception Capture](#exception-capture)
  - [Try-Catch Blocks](#try-catch-blocks)
  - [Finally Blocks](#finally-blocks)
- [Part 11: Advanced IO and Redirection](#part-11-advanced-io-and-redirection)
  - [Complex Redirections](#complex-redirections)
  - [Using File Objects](#using-file-objects)
  - [Understanding IO Ports](#understanding-io-ports)
- [Part 12: Interactive Features](#part-12-interactive-features)
  - [Command History Navigation](#command-history-navigation)
  - [Directory History Navigation](#directory-history-navigation)
  - [Customizing the Prompt](#customizing-the-prompt)
- [Part 13: Scripting Techniques](#part-13-scripting-techniques)
  - [Background Tasks](#background-tasks)
  - [Working with Both Values and Bytes](#working-with-both-values-and-bytes)
  - [Effective External Command Integration](#effective-external-command-integration)
- [Part 14: Practical Project - Simple Task Manager](#part-14-practical-project---simple-task-manager)
- [Part 15: RC Files and Customization](#part-15-rc-files-and-customization)
  - [RC File Organization](#rc-file-organization)
- [Part 16: Advanced Language Features](#part-16-advanced-language-features)
  - [Language Pragmas](#language-pragmas)
  - [Namespace Manipulation](#namespace-manipulation)
  - [Handling Circular Dependencies](#handling-circular-dependencies)

## Introduction to Elvish

Elvish is a modern shell that combines the interactive features of traditional Unix shells with a more consistent and powerful programming language. It's designed to be intuitive while offering advanced capabilities for both everyday terminal use and scripting.

## Part 1: Getting Started with Elvish

### Installing Elvish

First, you'll need to install Elvish on your system. Visit [elv.sh/get](https://elv.sh/get/) for installation instructions for your operating system.

Once installed, you can start Elvish by typing `elvish` in your terminal:

```bash
$ elvish
~>
```

The `~>` prompt indicates that Elvish is running and ready for your commands. The `~` shows your current directory (in this case, your home directory).

### Your First Commands

Let's start with some basic commands:

```elvish
~> echo Hello, World!
Hello, World!

~> + 2 3
▶ (num 5)
```

In the first example, `echo` is a command that prints its arguments.

In the second example, `+` is actually a command that adds numbers. Unlike most programming languages where math operators come between operands, Elvish uses a prefix notation where the command comes first, followed by its arguments.

The `▶` symbol indicates that the command is outputting a value, not just plain text. In this case, `(num 5)` is a numeric value.

## Part 2: Variables and Data Types

### Working with Variables

Variables in Elvish are defined using the `var` command:

```elvish
~> var name = World
~> echo Hello, $name!
Hello, World!
```

Notice that we use `$` to reference the variable's value.

### Lists

Lists in Elvish are created using square brackets:

```elvish
~> var fruits = [apple banana orange]
~> echo My favorite fruit is $fruits[0]
My favorite fruit is apple
```

Lists are indexed starting from 0, and you can access elements using square brackets.

### Maps

Maps (key-value pairs) also use square brackets but with `&` before keys:

```elvish
~> var person = [&name=Alice &age=30 &city=Wonderland]
~> echo $person[name] is $person[age] years old
Alice is 30 years old
```

## Part 3: Control Structures

### Conditional Statements

Elvish uses the `if` command for conditional execution:

```elvish
~> var x = 10
~> if (> $x 5) {
     echo "x is greater than 5"
   } else {
     echo "x is not greater than 5"
   }
x is greater than 5
```

### Loops

You can iterate over lists using the `for` loop:

```elvish
~> for fruit [apple banana orange] {
     echo "I like $fruit"
   }
I like apple
I like banana
I like orange
```

## Part 4: Pipelines and IO

Elvish supports both traditional byte pipelines and its unique value pipelines.

### Traditional Byte Pipelines

Like in other shells, you can pipe command outputs:

```elvish
~> echo "hello world" | grep hello
hello world
```

### Value Pipelines

What makes Elvish special is its ability to pass structured values through pipelines:

```elvish
~> use str
~> str:split , "apple,banana,orange" | str:join " and "
▶ 'apple and banana and orange'
```

This passes the list of strings from `str:split` directly to `str:join` without converting to text in between.

## Part 5: Function Definitions

You can define your own functions with `fn`:

```elvish
~> fn greet {|name|
     echo "Hello, $name!"
   }
~> greet "Elvish Learner"
Hello, Elvish Learner!
```

## Part 6: Working with Files and Directories

### Navigating Directories

```elvish
~> cd /tmp
/tmp> pwd
/tmp
```

### Directory Navigation Shortcuts

Elvish has a built-in location mode - press Ctrl+L to see and quickly navigate to recently visited directories.

### File Operations

```elvish
~> echo "test content" > test.txt
~> cat test.txt
test content
```

## Part 7: Advanced Value Types and Data Manipulation

### Typed Numbers vs String Numbers

In Elvish, numbers can be represented either as strings or as typed numbers:

```elvish
# String numbers (default when typing literals)
~> var x = 10
~> put $x
▶ 10

# Typed numbers (using the num command)
~> var y = (num 10)
~> put $y
▶ (num 10)
```

The difference becomes important in certain contexts:

```elvish
# Sorting behaves differently
~> put 1 12 2 | order
▶ 1
▶ 12
▶ 2  # Lexicographic string ordering

~> put (num 1) (num 12) (num 2) | order
▶ (num 1)
▶ (num 2)
▶ (num 12)  # Numeric ordering

# JSON conversion is different
~> put 10 | to-json
"10"  # String in JSON

~> put (num 10) | to-json
10  # Number in JSON
```

### Rational and Floating-Point Numbers

Elvish supports rational and floating-point numbers:

```elvish
# Rational numbers
~> num 1/3
▶ (num 1/3)
~> + 1/3 1/6
▶ (num 1/2)

# Floating-point numbers
~> num 3.14
▶ (num 3.14)
~> * 2.5 4
▶ (num 10.0)
```

Elvish preserves exactness in calculations when possible:

```elvish
# This stays as a rational (exact)
~> + 1/3 2/3
▶ (num 1)

# This becomes a float (inexact)
~> + 1/3 0.1
▶ (num 0.43333333333333335)
```

### Complex Data Structure Operations

Nested data structures are powerful for organizing information:

```elvish
# Creating nested structures
~> var people = [
     [&name=Alice &skills=[coding design] &contact=[&email=alice@example.com]]
     [&name=Bob &skills=[music writing] &contact=[&email=bob@example.com]]
   ]

# Accessing deeply nested elements
~> echo "Alice's email is "$people[0][contact][email]
Alice's email is alice@example.com

# Updating nested elements
~> var person = $people[0]
~> set person[skills] = [(all $person[skills]) painting]
~> put $person[skills]
▶ [coding design painting]
```

## Part 8: Modules and Code Organization

### Exploring Built-in Modules

Elvish comes with several built-in modules:

```elvish
# String operations
~> use str
~> str:to-upper "hello"
▶ HELLO

# Math operations
~> use math
~> math:pow 2 8
▶ (num 256)

# Path operations
~> use path
~> path:ext "document.pdf"
▶ .pdf
```

### Creating Your Own Module

Create a file named `~/.config/elvish/lib/greetings.elv` (Unix) or `%APPDATA%\elvish\lib\greetings.elv` (Windows):

```elvish
# greetings.elv
fn hello {|name|
  echo "Hello, "$name"!"
}

fn goodbye {|name|
  echo "Goodbye, "$name"!"
}

var default-name = "friend"
```

Now you can use your module:

```elvish
~> use greetings
~> greetings:hello "World"
Hello, World!
~> greetings:goodbye $greetings:default-name
Goodbye, friend!
```

### Module Search Paths

Elvish looks for modules in several directories:

```elvish
# View the module search paths
~> put $cmd:module-candidate-path
▶ ~/.config/elvish/lib
▶ ~/.elvish/lib
```

You can add your own paths:

```elvish
# In your rc.elv, add a path
set cmd:module-candidate-path = [
  $@cmd:module-candidate-path
  ~/my-elvish-modules
]
```

## Part 9: Function Programming

### Anonymous Functions (Lambdas)

Create functions on the fly:

```elvish
# Simple lambda
~> var greeter = {|name| echo "Hi, "$name"!"}
~> $greeter "Elvish User"
Hi, Elvish User!

# Used in a pipeline
~> put 1 2 3 | each {|x| * $x 10}
▶ (num 10)
▶ (num 20)
▶ (num 30)
```

### Closures

Functions capture variables from their environment:

```elvish
~> fn make-counter {
     var count = 0
     fn {
       set count = (+ $count 1)
       put $count
     }
   }
~> var counter = (make-counter)
~> $counter
▶ (num 1)
~> $counter
▶ (num 2)
```

### Higher-Order Functions

Functions that take functions as arguments:

```elvish
# Map (built in as 'each')
~> fn my-map {|f @items|
     for item $items {
       $f $item
     }
   }
~> my-map {|x| * $x $x} 1 2 3 4
▶ (num 1)
▶ (num 4)
▶ (num 9)
▶ (num 16)

# Filter
~> fn filter {|pred @items|
     for item $items {
       if ($pred $item) {
         put $item
       }
     }
   }
~> filter {|x| > $x 2} 1 2 3 4
▶ (num 3)
▶ (num 4)
```

## Part 10: Exception Handling

### Exception Capture

Use `?()` to capture exceptions:

```elvish
# Capture division by zero error
~> var result = ?(/ 10 0)
~> if (eq $result $ok) {
     echo "Calculation succeeded"
   } else {
     echo "Calculation failed: "$result[reason]
   }
Calculation failed: [&type=fail &content="division by zero"]
```

### Try-Catch Blocks

More complex error handling:

```elvish
~> fn safe-div {|a b|
     try {
       if (== $b 0) {
         fail "division by zero"
       }
       put (/ $a $b)
     } catch e {
       echo "Error: "$e[reason][content]
       put 0  # Default value
     }
   }
~> safe-div 10 2
▶ (num 5)
~> safe-div 10 0
Error: division by zero
▶ (num 0)
```

### Finally Blocks

Ensure cleanup regardless of errors:

```elvish
~> fn with-file {|filename fn|
     var file = (open $filename)
     try {
       $fn $file
     } finally {
       $file:close
       echo "File closed"
     }
   }
```

## Part 11: Advanced IO and Redirection

### Complex Redirections

Combine multiple redirections:

```elvish
# Save stdout to a file and error to another file
~> some-command > output.txt 2> error.txt

# Redirect both stdout and stderr to the same file
~> some-command > combined.log 2>&1

# Read from one file and write to another
~> cat < input.txt > output.txt
```

### Using File Objects

Working with file objects directly:

```elvish
~> var log = (path:temp-file)
~> echo "Starting log" > $log
~> echo "More entries" >> $log
~> cat $log
Starting log
More entries
```

### Understanding IO Ports

Elvish has numbered ports beyond the standard 0, 1, and 2:

```elvish
# Redirect to port 3
~> fn with-custom-port {|cmd|
     var file = (path:temp-file)
     $cmd 3> $file
     cat $file
     rm $file
   }
~> with-custom-port {|cmd| echo log message >&3 }
log message
```

## Part 12: Interactive Features

### Command History Navigation

Press Ctrl-R to search command history interactively.

### Directory History Navigation

Press Ctrl-L to access the location mode:

```
// You'll see an interface like this:
~>                                          user@host
 LOCATION
  5 ~/projects
  3 ~/documents
  2 ~/.config/elvish
```

### Customizing the Prompt

Modify your prompt in `~/.config/elvish/rc.elv`:

```elvish
# Custom prompt showing git branch
use path
use re
fn git-branch {
  var output = ?(git branch --show-current 2>/dev/null)
  if (eq $output $ok) {
    put " ("$output")"
  } else {
    put ""
  }
}

# Set a custom prompt
set edit:prompt = {
  var pwd = (pwd)
  var dirname = (path:base $pwd)
  put $dirname(git-branch)" > "
}
```

## Part 13: Scripting Techniques

### Background Tasks

Run commands in the background:

```elvish
# Start a background task
~> sleep 10 &
~> echo "We can continue working"
We can continue working
```

### Working with Both Values and Bytes

Combine byte and value pipelines:

```elvish
# Convert bytes to values
~> echo "1,2,3" | str:split , (slurp) | each {|n| * (num $n) 2}
▶ (num 2)
▶ (num 4)
▶ (num 6)

# Convert values to bytes for external commands
~> put foo bar baz | to-lines | grep a
bar
baz
```

### Effective External Command Integration

```elvish
# Working with JSON from external commands
~> curl -s https://api.example.com/data | from-json | get items | each {|item|
     echo "Processing "$item[name]
     # Do something with each item
   }

# Creating a shell function that wraps an external command
~> fn git-browse {|repo|
     var url = "https://github.com/"$repo
     echo "Opening "$url
     xdg-open $url # or 'open' on macOS, 'start' on Windows
   }
```

## Part 14: Practical Project - Simple Task Manager

Let's create a practical task manager:

```elvish
# ~/.config/elvish/lib/tasks.elv

# File to store tasks
var tasks-file = ~/.tasks.json

# Initialize tasks file if it doesn't exist
fn init {
  if (not (path:exists $tasks-file)) {
    echo [] > $tasks-file
  }
}

# Load tasks
fn load {
  init
  try {
    cat $tasks-file | from-json
  } catch {
    echo "Warning: Failed to load tasks. Initializing new list."
    put []
  }
}

# Save tasks
fn save {|tasks|
  put $tasks | to-json > $tasks-file
}

# Add a task
fn add {|description @priority|
  var prio = 'normal'
  if (> (count $priority) 0) {
    set prio = $priority[0]
  }

  var tasks = (load)
  var new-task = [&id=(count $tasks) &description=$description &priority=$prio &done=$false]
  save [(all $tasks) $new-task]
  echo "Added task: "$description
}

# List tasks
fn list {
  var tasks = (load)
  if (== (count $tasks) 0) {
    echo "No tasks."
    return
  }

  for task $tasks {
    var status = "[ ]"
    if $task[done] {
      set status = "[x]"
    }
    echo $status" #"$task[id]": "$task[description]" ("$task[priority]")"
  }
}

# Mark task as done
fn done {|id|
  var tasks = (load)
  var updated = $false

  for i (range 0 (count $tasks)) {
    if (== $tasks[$i][id] $id) {
      set tasks[$i][done] = $true
      set updated = $true
    }
  }

  if $updated {
    save $tasks
    echo "Marked task #"$id" as done"
  } else {
    echo "Task #"$id" not found"
  }
}

# Remove a task
fn remove {|id|
  var tasks = (load)
  var new-tasks = []
  var removed = $false

  for task $tasks {
    if (!= $task[id] $id) {
      set new-tasks = [(all $new-tasks) $task]
    } else {
      set removed = $true
    }
  }

  if $removed {
    save $new-tasks
    echo "Removed task #"$id
  } else {
    echo "Task #"$id" not found"
  }
}
```

Using our task manager:

```elvish
~> use tasks
~> tasks:add "Learn advanced Elvish" high
Added task: Learn advanced Elvish
~> tasks:add "Take backup of documents"
Added task: Take backup of documents
~> tasks:list
[ ] #0: Learn advanced Elvish (high)
[ ] #1: Take backup of documents (normal)
~> tasks:done 0
Marked task #0 as done
~> tasks:list
[x] #0: Learn advanced Elvish (high)
[ ] #1: Take backup of documents (normal)
```

## Part 15: RC Files and Customization

### RC File Organization

Well-organized `~/.config/elvish/rc.elv`:

```elvish
# === Module Imports ===
use str
use path
use math

# === Environment Setup ===
set E:EDITOR = "vim"
set E:PAGER = "less"
set E:PATH = (str:join : [
  $E:PATH
  ~/bin
  ~/.local/bin
]

# === Aliases & Helper Functions ===
fn l { ls -la $@ }
fn g { git $@ }
fn mkcd {|dir|
  mkdir -p $dir
  cd $dir
}

# === Key Bindings ===
set edit:insert:binding[Alt-x] = { echo "Executed custom command" }

# === Prompt Customization ===
# (As shown previously)

# === External Tool Integration ===
fn fzf-find {
  cd (fzf --height 40%)
}
```

## Part 16: Advanced Language Features

### Language Pragmas

Configure language behavior:

```elvish
# Set how unknown commands are handled
pragma unknown-command = external  # default, allows any external command
# Or for stricter environments:
pragma unknown-command = disallow  # requires explicit e: prefix for externals
```

### Namespace Manipulation

Working with namespaces as values:

```elvish
# Store a namespace in a variable
~> var math-ns = $math:
~> put $math-ns[pow](2, 10)
▶ (num 1024)

# Create a new namespace
~> var my-ns: = (ns [&x=100 &inc={|n| + $n 1}])
~> put $my-ns:x
▶ 100
~> $my-ns:inc 10
▶ (num 11)
```

### Handling Circular Dependencies

When modules need to reference each other:

```elvish
# module-a.elv
var greeting = "Hello"
use ./module-b

fn greet {|name|
  echo $greeting", "$name
}

fn complex-greeting {|name|
  module-b:format (greet $name)
}

# module-b.elv
use ./module-a

fn format {|greeting|
  str:to-upper $greeting"!"
}

fn say-hi {|name|
  module-a:greet $name
}
```

Elvish handles these cycles elegantly, unlike many other languages that would error on circular imports.
