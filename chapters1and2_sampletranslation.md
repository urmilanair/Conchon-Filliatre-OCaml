Conchon et Filliâtre 


# Programming in OCaml - On Algorithms and Data Structures

# Chapter 1. The Working Environment


The OCaml language is available for various operating systems (Linux, Mac OS, Windows, etc.). It can be obtained at the following address:
http://caml.inria.fr/ocaml/release.fr.html 
It is also available through the main package managers (apt-get, port, yum, brew, etc.).
OCaml can also be downloaded using the OPAM package manager, available at the following address: http://opam.ocaml.org 

## 1.1 Compiler and interpreter 

The OCaml language comes with two compilers: ocamlc and ocamlopt. The first generates portable code, independent of the hardware architecture; the second generates more efficient native code (X86, ARM, etc.). You can use either of these two tools to compile the programs given in this book. There is also an interactive version of the OCaml language: the ocaml program, an interpreter or toplevel, which executes a read-eval-print loop (REPL).

## 1.2 Our First OCaml Program 

We illustrate the use of these tools with a simple program, which consists in displaying the message Hello world!  on the screen. It can be written as follows: 

```
let () = print_string "Hello world!\n"
```

This is already a complete program. (We will go into the details of such programs in the next chapter.) If it is stored in a file hello.ml, it can be compiled from a terminal  with the compiler `ocamlc` as follows: 

```
> ocamlc hello.ml
```

We then execute the resulting binary, that is, a.out, with the expected result: 

```
>  ./a.out 
Hello world!
```

If we wish, we can give the binary a different name with the option `-o` of the compiler: 

```
> ocamlc -o hello hello.ml 
> ./hello 
Hello world! 
```

To use the interpreter, it suffices to type the command `ocaml` in a terminal. The program then displays a prompt in the form of the character `#`, inviting the user to enter an expression.

```
> ocaml
OCaml version 4.00.1 
# 
```

To be evaluated, the expression must end with two semicolons `;;` followed by a carriage return. The toplevel verifies that the expression is syntactically correct and well-typed. It then evaluates the expression and displays the result. All you have to type is: 

```
> ocaml 
OCaml version 4.00.1 
# let () = print_string "Hello world!\n";; 
Hello world!
# 
```

And the message `Hello world!` is displayed on the screen.

In addition to the result, the toplevel also displays the type inferred by OCaml for the expression entered. For example, if we enter the mathematical expression 1 + 4 × 2, the interpreter gives us the following response:

```
# 1 + 4 * 2 ;; 
- : int = 9 
# 
```

This shows that the expression is of type `int` (the integer type), and that its result is 9. From now on, we will represent the expression entered by the user and the interpreter’s response against a grey background. Thus, the evaluation of the preceding expression will be represented as follows:

```
# 1 + 4 * 2 ;  ; 
- : int = 9 
```

To exit the toplevel, type the command `#quit` as follows: 

```
# #quit;; 
>
```

A simpler way to do this is to press the keys `ctrl` and `D` simultaneously.


## 1.3 Programming Environments 

As with many other programming languages, you can work with OCaml in your prefered text editor (Emacs, gedit, Aquamacs, etc.). You can, if you like, configure the text editor to get OCaml-specific syntax highlighting, error locations, and so forth.

The easiest way to automate the compilation process is to use the tool `ocamlbuild`, which is included in the OCaml distribution. Another solution, with the tool GNU `make`, requires the writing of an OCaml-specific configuration file (`Makefile`).

An alternative to these solutions is to use an integrated development environment (IDE). For example, there is an Eclipse plugin for OCaml, OcaIDE, available at: http://www.algo-prog.info/ocaide/ 

## 1.4 Installing OCaml Libraries

The community of OCaml users has developed many OCaml libraries. The easiest way to install them is to use the package manager OPAM, whether or not OPAM was used to install OCaml.

## Online Environments 

There are also online solutions to use OCaml without installing it on your machine. One example is the online interpreter TryOCaml, available at this address: http://try.ocamlpro.com/ 


 
# Chapter 2. First Steps with OCaml

The best way to learn a programming language is by programming. We therefore present fourteen (little) programs to get started with OCaml. The aim is to introduce the basic constructions and ideas of the language.

## 2.1 Leap Years 

Ideas Introduced 
- the general form of a program
- the let construction
- calling a function
- basic types (`int`, `bool`, `string`) 
- libraries (`Printf`, `Sys`, etc.), module system, open directive 
- access to program arguments (`Sys.argv`) 
- access to the elements of an array (`t.(i)` notation) 

### Program 1 [leap_year.ml] — Leap Year

```
let year = read_int () 

let leap = 
  (year mod 4 = 0 && year mod 100 <> 0) || year mod 400 = 0 

let msg = if leap then "is" else "is not" 

let () = Printf.printf "%d %s a leap year\n" year msg 
```

Our first program, `leap_year.ml`, determines if a given year is a leap year. We can compile it using `ocamlc`.

```
> ocamlc -o leap_year leap_year.ml
```

Once you launch the program, it waits for you to type in a year. Once you do this, it tells you if the year is a leap year.

```
> ./leap_year
2013
2013 is not a leap year 
```

Let us now consider the structure of this program. The first line reads an integer from standard input:

```
let year = read_int ()
``` 

This line alone contains several notions of the OCaml language. Firstly, it corresponds to a variable binding of the form: 

```
let  year  = ...
```

This serves to define a (new) variable `year` with the result of the expression to the right of the symbol `=`. Here, the type of the variable `year` does not have to be declared; it is automatically deduced by the compiler or the interpreter. The expression to the right of the symbol `=` is a function call. It is a call to the predefined function `read_int`.

```
... = read_int ()
```

Here, `()` indicates that the function `read_int`  does not take any meaningful argument. It only returns an integer, read from standard input. 

The second line introduces a Boolean variable `leap` that is true if and only if the year `year` is a leap year, that is, if it is divisible by 4 but not by 100, or if it is divisible by 400.

```
let leap =
  (year mod 4 = 0 && year mod 100 <> 0) || year mod 400 = 0 
```
  
The infix operator `mod`  gives the remainder of integer division. The operators `&&` and `||` represent the Boolean AND and OR, respectively. As with `year`, the type of the variable `leap` is deduced automatically. The following line introduces a third variable, `msg`, which contains a string: 

```
let msg = if leap then "is" else "is not" 
```

The variable `msg` therefore contains the string `"is"` if `leap` is true, and `"is not"` if it is false. (Strings are delimited by quotation marks `"`.) Note that the construction `if-then-else` is used here to construct an expression, namely, a string.

Finally, the last line displays a message that indicates whether or not `year` is a leap year.

```
let () = Printf.printf "%d %s a leap year\n" year msg 
```

The display is achieved by a call to the library function `Printf.printf` with three arguments: a formatting string `"%d %s a leap year\n"`  and two values `year` and `msg` that will respectively replace `%d` and `%s` in the printed message. The function call is denoted by a simple juxtaposition of the function and its arguments. Unlike other languages, OCaml does not use parentheses around arguments.

```
... = Printf.printf "%d %s a leap year\n" year msg 
```

We refer the reader to the OCaml manual for further information on the standard library and, in particular, on the function `printf` of the `Printf` module. The manual is available online at the address http://caml.inria.fr/pub/docs/. Finally, note that the call to the function printf is contained in a statement with a specific form: 

```
let () = ...  
```

This is somewhat enigmatic, and will be left so for the present. It will be explained when we discuss pattern matching in sections _2.6 Drawing a Curve_ and _2.12 Playing a Musical Score_. For the present, suffice it to say that, in general, an OCaml program is a series of statements that are evaluated from top to bottom, that is, in the order in which they appear in the source file. Unlike languages like C or Java, there is no `main` entry point. The statement `let () = ...`  is used in the case of expressions that do not return a value so as to maintain the same format. In this case, OCaml verifies that the expression does not have a value.

