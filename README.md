# Learning C

* [Getting Started](#getting-started)
* [Variables](#variables)

## Getting Started

C is a compiled language, which means that the source code needs to be translated into a set of CPU instructions for your processor. That means whatever code you write, you have to compile it into an executable, then run that executable to run your code. The compiler we will be using is `gcc`, which is one of the most popular C compilers. 

Every C program starts from the `main` function. This function should look like this (we will go into the syntax later)
```C
int main() {
  // Program goes here
  return 0;
}
```

To write a "Hello World" program, we have to add a couple things that we'll go into later, but we want to get a working executable for now.
```C
#include "stdio.h"
int main() {
  printf("Hello World\n");
  return 0;
}
```

Say we put this code into a file named `test.c`. We could then compile that source code file into an executable using
```
gcc test.c -o test
```

This compiles the code in `test.c` and creates an executable (named `test` in this command), than we can then run and it will print "Hello World".
```
$ ./test
Hello World
```

Now, we have an idea about how to compile and run a C program. It's time to go more into the language itself.

## Variables

Variables in C are important to understand, like in any language. Every variable requires a type that specifies the kind of data that it holds. The most basic type is an `int`, which specifies an integer type. To declare a variable named `foo` with type `int`, we do
```C
int foo;
```

We can specify an initial value for this variable by doing
```C
int foo = 3;
```

Variable names can only start with letters or an underscore, and after than can only contain letters, numbers, and underscores. Typical naming convention is camel case, which means every word should start with a capital besides the first. For example if I wanted a bank account balance variable, the proper way to name that would be `bankAccountBalance`. This makes your code more readable and coherent.

### Variable Types

An important part of learning C is learning the types. C does not have classes like C++ or Java, which makes it fairly simple.
* [Numeric](#numeric)
* [Arrays](#arrays)
* [Pointers](#pointers)
* [Structs](#structs)

#### Numeric

C contains a few numeric types: `char`, `short`, `int`, and `long`. The difference between these is the size of the variable, in bits. A `char` is 8 bits, a `short` is 16 bits, `int` is 32 bits, and `long` is 64 bits (if you're really worried about these sizes, though, you should be aware of some limitations due to legacy machines, and look into using types defined in stdint.h). For the most part, we can just use an `int` as a regular signed integer, and `char` as a character. 

C contains two floating point number types: `float`, which is a 32 bit floating point number, and `double`, which is a 64 bit floating point number.

#### Arrays

Arrays are a important part of C. They have a fixed length, and can be any type. To declare an array variable of type `T` and length `N`, we would use
```C
T variableNameHere[N];
```

So an example would be, if we wanted to make an array of integers, length 10, named `myNums` it would look like this
```C
int myNums[10];
```

Or an array of floats, length 128, named `measurements`
```C
float measurements[128];
```

We can initialize this array using the `{}` characters. Keep in mind that you should only initialize as many elements as you have defined in the array. We can specify the elements by separating them with a comma. 
```C
int myNums[3] = {2,4,8};
```

If you are declaring an array with an initializer, you can actually leave out the size of the array and the compiler will infer it from the number of arguments specified.
```C
int myNums[] = {1,2,3,4};
```

**Know that this array initialization only applies when declaring an array for the first time! You cannot use this on an existing array to redefine its elements!**

#### Pointers

Pointers are the heart of C. A pointer, simply put, is a reference to another variable or value. We will discuss this topic more when we get to how memory works, but for now we will just go over how to declare a pointer. 

The important thing to know is that a pointer must know what type of value it points to. A pointer that references a `float` will behave differently than a pointer that references a `char`. We can declare pointers using the `*` symbol in between the type and variable name.

So, to declare a pointer to an `int`, we would write
```C
int *myPointer;
// It doesn't matter where the * goes, so below is also valid
int* myOtherPointer;
```

Or to declare a pointer to a `char` we would write
```C
char *myCharPointer;
```

We can combine the previous types, by creating an array of pointers. This is getting a little more advanced, but it's pretty easy to understand syntactically. An array is just simply a block of values, no matter whether the type is an `int` or a pointer to an `int` or whatever else. 
```C
int *myIntPointerArray[10];

double *anotherArray[20];
```

Again, you don't need to understand why this is important right now, but this is how you write it in C.

#### Structs

Structs are how data is organized in C. All a struct is is a specific, named collection of values. This allows us to group related data into a single type which makes our code cleaner and easier to write. 

To declare a struct, we can use the `struct` keyword like so
```C
struct MyStruct {
  // Contents of struct
};
```

The naming convention here is similar to the variable naming convention, but you should capitalize the first letter. Again this isn't required, but it makes your code cleaner and more readable.

Now, an empty struct is useless. We want it to contain useful data. To do this, we declare "members" of the struct inside of the curly braces. Each member has a type and a name. Each member is separated by a semicolon.
```C
struct MyStruct {
  int memberA;
  float memberB;
  char *memberC;
};
```

Now `MyStruct` is defined as a struct that contains three members: an `int` named `memberA`, a `float` named `memberB` and a `char*` named `memberC`.

**This struct is just a template! Declaring a struct does not create a variable for that struct!** This is an common misconception. By defining a struct, we have created a new type, not a variable. What this means is essentially we have created a template for future use. If we need a collection of a `int`, `float`, and `char*` all together, we can use this struct. We would create a variable whose type is `MyStruct`.

To use a struct, we use it as the type of a variable like so
```C
struct MyStruct myVar;
```

We have just created a variable named `myVar`, with type `MyStruct`. The `struct` keyword is just there to inform the compiler that we are referencing a `struct` named `MyStruct`. Now `myVar` contains the three types we've specified in the struct. We can access them using their names.
```C
myVar.memberA;
myVar.memberB;
myVar.memberC;
```

Remember, these have types. Every time we refer to `memberA`, the compiler knows we are referring to an `int`, and same with the rest. `myVar` is simply just a collection of these values. 

#### An Example

Let's walk through an example to tie these together. Say we want to keep track of bank account information. We can make a struct that will encapsulate the values that should be in an account.
```C
struct BankAccount {
  int accountId;
  float accountBalance;
};
```

That's pretty simple. Now we have a struct as a template for whenever we want to make a BankAccount. Say we want our bank to be able to hold 1000 accounts. We can make an array of `BankAccount` with length 1000 to hold our bank information.
```C
struct BankAccount bankAccounts[1000];
```
--A
Now we have an array containing 1000 bank accounts, and each one we can access its `accountId` and `accountBalance`. We will get into how to use these things later, but it is important to understand how the types work. 
