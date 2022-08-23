# Learning C

* [Getting Started](#getting-started)
* [Variables](#variables)
* [Pointers] (#pointers)

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

#### Pointer Types

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

## Pointers

Pointers are at the heart of programming, and they allow you as the programmer to have control over the memory that you are using. 

### How does Memory Work?

Almost every variable that you use is stored somewhere in memory. The computer has to have somewhere to get the data from before operating on it. For example, if we make an array of 1000 bank accounts, those are stored in a large section of your computer's memory. When you want to access a particular bank account from that array, the computer fetches the data from that section of memory. That leads us to the question: _How does the computer know where in memory to get the data?_

### The Concept of a Pointer

Because we know that we have stored data in memory, now we have to be able to reference it. Each memory location has an address that uniquely identifies that section of memory for your computer. These addresses are organized by bytes, so each byte has a unique address. Knowing these, we can see that each byte in memory has two values associated with it: its value and its address. We can imagine these as mailboxes.

Say you are in a mail room with several numbered mailboxes. Each mailbox's number uniquely identifies that mailbox, and each mailbox holds a different package. You know that your package is stored in mailbox 11, so you go and open up that mailbox and get your package. This is very similar to what is happening inside your computer. The number of the mailbox is the address of that part of memory, and the package represents the value inside that mailbox. 

### Pointer Types (cont.)

Earlier we discussed pointer types. Each pointer is a reference to a memory address, but the type must contain what we are pointing to. This is because different types have different sizes. A `char` takes only 1 byte, but an `int` takes 4 bytes. So if we have a pointer to a `char`, that points to a single byte in memory. Conversely if we have a pointer to an `int`, it points to a contiguous 4 byte section of memory where that `int` is stored. The compiler has to know how big of an area of memory your pointer is pointing to.

### Using Pointers

Now it's time to actually put pointers to use.

#### Allocating Memory

If we want to use memory, we will have to first allocate it. This means that we reserve a certain section of memory for our program where we can store our data. We can do this as many times as we want, with however big sections that we want until the computer runs out of memory. Because of certain practical and security reasons, the computer does not allow you to access whatever memory you want, and allocation is a way to get your own section of memory.

This is done by using the standard library function `malloc`. The argument to malloc is the how much memory we want to allocate, in bytes. `malloc` returns a pointer that points to the beginning of this section. 
```C
char *myPointer = malloc(1024);
```
The above code is a simple line of code that allocates 1024 bytes, then stores the pointer to this section in the `myPointer` variable. An important note here-malloc doesn't require a certain pointer type. We have a 1024 byte area of memory, and it's up to us how we want to section that. The most basic pointer is a `char*` because it simply points to a single byte in memory. However, usually the reason for which we are allocating memory dictates what type we want to point to. 

#### Getting the Address of Variables

Like I said earlier, the variables that you declare are also in memory. Just like any other value, they have an address that uniquely identifies that value's location in memory. We can get this using the `&` operator. The `&` operator is applied to a variable in C, and it provides us with a pointer to that object (note: the pointer to this object _does_ have a specific type. Because we are getting the address of a specific type, it must be a pointer to that type. Using `&` on an `int` will always provide an `int*`).
```C
float f = 1.2;
float *pointerToF = &f;
```
The above code shows that, given a variable `f`, we can use the `&` operator to get a pointer to `f`. We know that since `f` is a `float`, it is a 4 byte value containing the number `1.2`. Our variable `pointerToF` is simply holds the address of that 4 byte area in memory containing `1.2`. 

Going back to our mailbox analogy, this operator gives us the mailbox number. Say there is a package that we know is in one of the mailboxes, but we aren't sure where. If we can get the mailbox number (the address), now we know _where_ the package is stored. 

#### Dereferencing

Now, a pointer stores the address of the value it points to. So if we were to print the pointer, we would get a large number (which is in fact a memory address). So great- we have a variable that stores a memory address. That isn't much use to us unless we can access the value at that address in memory. Going back to our mailbox analogy, if a pointer is simply the mailbox number, it isn't much use if we can't get the package out of the mailbox at that number. Dereferencing a pointer is the concept of accessing the value that is pointed at by the pointer. Dereferencing is getting the package out of the mailbox with a given number. We can take a pointer to a type and get the object at that location in memory. For example, if we dereference an `int*`, it will give us an `int`. This is done using the `*` operator before the address (this is different than the `*` used in the type declaration; they are the same character but are not related).
```C
int myVar = 64;
int *pointerToMyVar = &myVar;
int dereferencedValue = *pointerToMyVar;
```
The above code declares an `int` that holds the value 64, and gets a pointer to that variable. We use the `*` in the third line to dereference this pointer, and store the actual value being pointed at into a third variable. What the computer does here is gets the value of `pointerToMyVar` (which is the address of `myVar`). It then looks up that address in memory, and accesses those 4 bytes (4 bytes because it is an `int*`). We know that `64` was stored in that section of memory to begin with, so when the computer looks up that address in memory, it fetches the value 64, like we expected. Therefore `dereferencedValue` also stores 64. 

Now- a quick note: because we declared a new variable `dereferencedValue`, that means that variable is stored in a different memory location from `myVar`. So they are two different variables that contain the same value, `64`. Dereferencing fetches the value then stores it in the new variable; there is no connection now between `dereferencedValue` and `myVar`. 

#### Pointers to Structs

Using pointers in combination with structs is one of the powers of C. If we have a pointer to a struct, it is no different than a pointer to any other type. It is simply the address of that struct object in memory. Lets bring back our bank account example from before.
```C
struct BankAccount {
  int accountId;
  float accountBalance;
};
```
We have our BankAccount struct that is 8 bytes long (4 bytes for an `int` and 4 bytes for a `float`). So if we had a pointer to this:
```C
struct BankAccount *bankAccountPtr;
```
It contains the address of an 8 byte section of memory containing an `accountId` and `accountBalance`. 

If we want to access the members of this struct we have two options. We'll start with the naive option.
```C
struct BankAccount dereferencedAccount = *bankAccountPtr;
```
This simply dereferences the struct and stores it in a new variable called `dereferencedAccount`. This is fairly intuitive, and it does what we want it to do: we can access the values of this struct by accessing the `accountId` and `accountBalance` of `dereferencedAccount`. However this can get problematic when we have much larger structs where we only need to access a few values in that struct. We don't want to make space for a whole new dereferenced struct of that type, so we can use a method to get those values from the pointer directly. 

```C
int myId = bankAccountPtr->accountId;
```
Using the `->` instead of the `.` operator, we can access members of the struct pointer like we access members of a struct. When we use the `->` operator, the computer looks up that struct in memory (using the address in the pointer), finds that member's value in the struct (in this case, it fetches the appropriate 4 byte `int` inside our struct), then provides that value back to the program (in this case, storing it in a new variable `myId`). 

Not only is this more convenient than dereferencing the full struct, we can also use this to _set_ the member values.
```C
bankAccountPtr->accountBalance = 100.0;
```
This is actually storing the value `100.0` into this struct in memory. Again using the address in `bankAccountPtr`, the computer stores this value into the correct memory location so that this struct's `accountBalance` now contains the value `100.0`.

*Important*: we cannot set values of a dereferenced struct. This makes sense when you stop to think about what the computer is actually doing when you dereference a struct. When we do 
```C
struct BankAccount dereferencedAccount = *bankAccountPtr;
```
We are accessing the struct from memory, and copying it into a new variable `dereferencedAccount`. This new variable is entirely separate from the pointed-to value, and thus is at a different location in memory. So if I were to do
```C
dereferencedAccount.accountBalance = 100.0;
```
It would set the `accountBalance` for `dereferencedAccount`, but would have no effect on the value pointed to by `bankAccountPtr`. 

#### Pointer Arithmetic

TODO

#### Why Pointers?

Pointers allow us as the programmer to have fine-grained control over the memory that we are using. But you may be wondering why they are useful. In these examples so far, we haven't really seen a need for pointers. They start to get quite useful once we start using functions and our program gets bigger. We want to be able to use certain important values in multiple places throughout our program. If multiple functions in our program can reference the same value in memory, it greatly expands what we can do with that value and what we can do with our program on a larger scale. 
