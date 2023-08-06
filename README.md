# Clean code notes
### Recommended books:
Robert C. Martin - **Clean Code: A Handbook of Agile Software Craftsmanship** \
Kent Beck - **Implementation Patterns** \
Grady Booch - **Object-Oriented Analysis and Design with Applications** \
Dan North - **97 Things Every Programmer should Know** \

[Clean Code - Uncle Bob / Lesson 1](https://www.youtube.com/watch?v=7EmboKQH8lM) \
[Clean Code - Uncle Bob / Lesson 2](https://www.youtube.com/watch?v=2a_ytyt9sf8) \
[ITT 2016 - Kevlin Henney - Seven Ineffective Coding Habits of Many Programmers](https://www.youtube.com/watch?v=ZsHMHukIlJY)

### Cleaning code
**When and how much time do you invest in cleaning your code?**
When you finished your feature and it’s working, that’s the moment when it’s time to clean it. **It will take roughly the same amount of time it took you to write it.** You are not done when it works, **you are done when it’s right.**

>“The only way to go fast is to go well.”
>- Robert C. Martin

>“Clean code is simple and direct. Clean code reads like well-written prose...” 
>- Grady Booch

>“Clean code always looks like it was written by someone who cares.” 
>- Michael Feathers

### Functions

#### The rules of functions:
- They should be small.
- They should be smaller than that.

#### How small should a function be?
- Functions should hardly ever be 20 lines long. **A function should do** one thing and **one thing only.**
- Therefore, the **indent level** of a function should not be greater than **one** or **two**.
- This makes the functions **easier to read and understand.**

### What's one thing?
A function does one thing, if you **cannot meaningfully extract an other function from it**. If a function contains code and you can extract an other function from it, your function did more than one thing. That means you should extract and extract and extract until you cannot extract anymore. Take all the functions in the system and explode them down into tiny little tree of small functions optimally extracted. \
```Tip: intelliJ have a refactoring menu with method extraction```

### But will I drown in a sea of tiny little functions?
No, you will not drown in a see of tiny little functions. Because you will going to give those functions **names**, and as you are naming them, you will move them to **appropriately named classes and packages**. You will create a semantic tree of functions you can follow by name.

### Function names
Function names should not be nouns, they should be **verbs** because **they do things.**

### Level of obstruction
Every line of a function should be at the same level of obstruction and that should be one below the name. \
#### Example for bad:
```java
WikiPage wikiPage = pageData.getWikiPage(); //High level
if (wikiPage != null) { //Low level
```

### Refactoring big functions
What are you doing when a part of a function changes 2 local variables? You cannot extract that!? Make those variables global and then you can extract them and you can extract a bunch of other functions after that. \
What do you call a set of functions that manipulates a set of variables? That is called a **class**. A class could hide in your big functions. \
This is how you can find the true object oriented structure of the system you are trying to design.

### Readability
A function should be **polite**. It should allow the user to **exit early**. The below example is one function call instead of 50 lines of code. It allows you to **understand** this part **in 3 seconds in a very high level**, not at the detailed level. You can look in it to see the details if you want, but it saves you a lot of time if this is enough for you to understand what is happening.

#### Example
```java
 renderpageWithSetupsAndTearDowns();
```
**If statement’s body should be a function call with a nice readable name** that tells you what the function is going to do. \
Inside **if statement’s parenteses should be a function call too** (applies for **try-catch also**)

#### Example
```java
if (employeeIsTooOld()) { \
fireEmployee() \
}
```

### Function arguments
#### How many arguments should a function have?
**Zero** is a good number, it is really easy to understand a function with zero arguments. \
**One** is not too hard to understand. \
**Two** is ok. The human brain is pretty good to keep two things in order. \
**Three** starts getting harder. There is 6 ways to order 3 arguments. \
**Four** arguments could be ordered in 24 ways. \
**Six**. If those six things are so cohesive, that they are going together into a function, why aren’t they an object already? \

We don’t want to see a long comma separated list of arguments. It’s rude. It’s hard to understand what’s going on in a couple of seconds.
Keep the number of arguments down to 2-3, use other strategies like data structures or create objects. \

**Don’t pass booleans to a function**. It means that there should be an if statement inside the function. Then the if statement have 2 branches, the normal and the else branch. Why not just separate them into 2 branches? You can pass booleans if you are setting a state of a switch, but dont use it to testing arguments, it’s just rude and annoying. You are not gonna read what the boolean does, you will think that “the author probably new what he was doing” and you will walk away.

## Names
### Boolean name example
```java
 **boolean** testPage = **true**;
```
This is an **explanatory variable**, it’s only job is to explain if something is a testpage. To make it look more like “a well written prose” in an if statement, **isTestPage would be a much better name.##

### Output arguments:
I couldn’t make it understandable by reading, whatch the video:
https://youtu.be/7EmboKQH8lM?t=4464
This part ends with this:
>The principal of least surprise: Make sure your code is not surprising.

### What's wrong with if statements?
- They do more than one thing

#### Example:
What if you have a switch statement that switches on a type of shape. Circle, square, etc…
How many switch statements would your system have if you are dealing with shapes?
At least:
- You have to deal with it when you are drawing the shape
- You have to deal with it when you are rotating the shape
- drag
- stretch
- etc…

In every single one you will have a switch statement. What goes wrong?
What happens when you are rotating the shapes?

With a circle, nothing, so the programmer does a logical optimization and here is the problem.
What happens when you add a new type of shape a rombus?
- you gotta find all the switch statements and update them in just the right place
- some of the might be if statements
- this is difficult, it is fragile and will break

#### What's the solution?
**Polymorphism of course!** \
We wanna have a base class called shape and subclasses for triangle, square, etc… Than you could put all of those functions into the derivatives. Draw, rotate, drage, stretch.

Now what happens when we are adding a type new shape? What changes in the system?
We will add a new file, a class, a new subclass but nothing else changes in the system. The switch statements are all gone.

This is the **open-closed principle**. A system or module should be **open for extension but closed for modification**. You should be able to **extend the module without modifying that module**. How do you do that? By **creating base classes and having derivatives**. Our system now can be extended to have new shapes without modifying the system. This is one of the reasons we don’t like switch statements.

### Command and query separation
**A function that returns void must have a side effect**. If it has no side effect, there is no point in calling it.

**Commands change the state of the system therefore they return void**. **Anything that returns a value, by convention will not change the state of the system**. That way when you see a function that returns a value, you know it’s safe to call it and will leave the system in the same state it was found in. This is a convention to follow, the language doesn’t force it of course. **It will help us keep tracking side effects.**

### DRY principle. Don’t repeat yourself.
// TODO

### Comments
**The purpose of a comment is to explain the purpose if the code can’t explain it’s own purpose.**

**Comments are a last resort.** Every comment represents a failure. A failure in expressing yourself in code.
Comments lie. They degrade over time because noone maintains them. Noone cares about them. Your IDE even paints it gray that shades into the background so it is easier to not give a duck about them.

When you write bad code, don’t comment it, clean it! It is much better to use the names of functions and variables to explain the code than it is to use a comment to explain the code.

TODO comments became DONT DO comments when you are checking it in. Noone will ever care about them. If you have TODO comments they must be done or deleted before you open a pull request.

### Names: Reveal your intent
```java
int d; // elapsed time in days
```

- A name that requires a comment, does not reveal it’s intent.
- The name of a variable should tell us the significance of what that variable contains:
```java
int elapsedTimeInDays;
```

### How long should names be?
A variable name should be proportional to the size of the scope contains it.
- If a scope is very small, like 1 line than a single letter is fine.
- Variable names inside a small while loop or if statement should be very short
- If you have a function that’s 4 lines long, variable names should be very short, but because it’s 4 lines long, arguments would maybe be a little bit longer. A word would be good.
- Instance variables inside a class have slightly longer scope of the class. They should be longish, 2 words maybe.
- Global variables have a huge scope. Their name should be very long.

Functions are exactly the opposite
- A funcion’s name is inversely proportional to the size of the scope that contains it.
- The bigger the scope the smaller the name
- We would not want to call the open() function if it’s name was: openFileAndThrowExceptionIfNotFound().
- As the scope of the function gets larger we want the name to shrink.
- We want it because we want to call it more. A function that lives in a large scope will be called from all of the place, so we want to shrink the name down.
- A function that lives in a large scope must be abstract. (Abstraction is the process of removing physical, spatial, or temporal details or attributes in the study of objects or systems to focus attention on details of greater importance)
- Private functions called by public functions will have longer names.
- Private functions called by private functions will have even longer names.
- As you are extracting functions while cleaning your code, every time you go down an other level, the function the names gets longer because the function gets more precise. It does something really tiny, really precise and you need words to specify.

Class names are like function names.
- Classes at the global scope have 1 word names
- Derived classes, inner classes have multiple word names
- As the scope shrinks the name grows

#### Example
```java
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int[]>();
    for (int[] x : theList)
        if (x[0] == 4)
            list1.add(x);
    return list1;
}
```

```java
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell : gameBoard)
        if (cell[STATUS_VALUE] == FLAGGED)
            flaggedCells.add(cell);
    return flaggedCells;
}
```

The names here tells you what’s going on inside the function. It’s getting all the flagged cells. This naming system does more than just make it easier to understand the function. It tells you what program contains the function. This program is some kind of a game, probably Mine Sweeper. A good naming system tells you about the entire context of the system. That’s the power of a good system of names.

Code can contain optical illusions. It’s your job, to fight against them.
>XYZControllerForEfficientHandlingOfStrings
>XYXControllerForEfficientStorageOfStrings

It’s really hard to see the difference. It is especially easy to look over the difference in the third character in these variable names. Be careful about that.

### Number series

The opposite of intentional naming.
Provide no clue as to the author’s intent.
```java
public static void copyChars(char[] a1[], char[] a2[]) {
    for (int i=0; i<a1.length; i++)
        a2[i]=a1[i];
}
```

```java
public static void copyChars(char source[], char[] destination[]) {
    for (int i=0; i<source.length; i++)
        destination[i]=source[i];
}
```

### Noise words

#### Suffixes:
Product product;
ProductData pd;
ProductInfo pi;

#### Prefixes
Product aProduct;
Product theProduct;


Data is completely redundant. Why do you need to say it’s data? Of course it’s data. You don’t need to put it there. What is the difference between Product and ProductData? Why is there 3 different objects if there is a Product object anyway? These type names don’t thell you the difference, and this is a problem.

#### Distinguish names meaningfully

```java
Account getActiveAccount();
List<Account> getActiveAccounts();
List<Account> getActiveAccountInfo();
```

How are the programmers in this project supposed to know which of these functions to call? What are the first function is returning? There could be more that one active account. The third function is even worse. It returns a list of accounts just like the second function when it should return “account info”. **The names are not telling you what’s going on here**.

How much time should you spend on code review?

If a feature takes 5 hours to complete, than you should take something like 5 hours or maybe less to review it. If you are going to review a module, **you need to walk through the reasoning the author originally went through.** Hopefully the author made it easy for you by refactoring and cleaning up the code but it will still take a significant amount of time to review the code.

### Noise in code:

### Argument list:

#### Bad:
```java
public int howNotToLayoutAMethodHeader(int firstArgument,
    String secondArgument)
```

#### Good:
```java
public int ensureArgumentsAreLikeThis(
    int firstArgument,
    String secondArgpublic int ensureArgumentsAreLikeThis(
    int firstArgument,
    String secondArgument)

public int orEnsureArgumentsAreGroupedLikeThis(
int firstArgument, String secondArgument)

public int orEnsureArgumentsAreGroupedLikeThis(
int firstArgument, String secondArgument)
```

#### Bad:
```java
public int butNotAlignedLikeThis(int firstArgument,
                                                      String secondArgument)
```

When we are dealing with a list of parameters than it should look like a list, it should not be scattered around. You can do it horizontally or vertically, but make it look like a list.

### Lego naming:

**We often dilute the meaning so much, it disappears.**
You have a ManagerController and you would need a proxy for it and a factory and it needs to be abstruct so now you have a: **AbstractFactoryProxyManagerController**.

This is not a particularly long name but this is a very good example to demonstrate where it starts when we are adding extra words.

```java
public interface ConditionChecker
{
        boolean checkCondition();
…
}
```

This is a condition and what you are trying to find out is whether or not it is true? So you mean that:
```java
public interface Condition
{
        boolean isTrue();
…
}
```

If you have seen the Matrix, there is a moment when Morheus says to Neo “stop trying to hit me and hit me”. Stop trying to dance around the name and say what it is.
The point here is not just the really long names are bad, but long names start small and we are starting to add clarifications and then we go over the hill. We lose clarity and end up with this noise.

An observation was made in the mid 90’s:
They have to tack on a flowery, computer science-y, impressive sounding, but ultimately meaningless word, like **Object, Thing, Component, Part, Manager, Entity, or Item**.
There is a brilliant metric to your names: **How much of a name is left when you remove all of these parts?** If there is nothing left it is definately not a good sign.

InvalidOperation

We have many habits and we are blind to them. We dn’t even see that we are doing them. That’s the point of habits, they are automated, it doesn’t need any thinking.

There has become a habit across a number of different languages to put the word **Exception** on exceptions. **What else would appear in a throw statement or catch clause?** This doesn’t make anything more clear. It’s an Exception by definition by it’s context.
In the same way we are not labeling our non Exception classes NonException.

Example:
```java
AccessViolationException
ArgumentOutOfRangeException
ArrayTypeMismatchException
BadImageFormatException
CannotUnloadAppDomainException
EntryPointNotFoundException
IndexOutOfRangeException
InvalidOperationException
```
These are actually good names, if we take away the Exception, the names have their force on their own, they carry the meaning of the concept:
```java
AccessViolation
ArgumentOutOfRange
ArrayTypeMismatch
BadImageFormat
CannotUnloadAppDomain
EntryPointNotFound
IndexOutOfRange
```

#### Example with bad names
```java
ArgumentException
ArithmeticException
ContextMarshalException
FieldAccessException
FormatException
NullReferenceException
ObjectDisposedException
RankException
TypeAccessException
```
If we take Exception off these doesn’t mean much:

```java
ArgumentException
Arithmetic
ContextMarshal
FieldAccess
Format
NullReference
ObjectDisposed
Rank
TypeAccess
 ```
It’s not because we need Exception, that’s because the name is bad to start with.

#### Clarified version:
```java
InvalidArgument
InvalidArithmeticOperation
FailedContextMarshal
InvalidFieldAccess
InvalidFormat
NullDereferenced
OperationOnDisposedObject
ArrayRankMismatch
InvalidTypeAccess
```

There is nothing wrong with NullReference, but dereferencing it is the problem. Name the problem. Name the situation. Don’t name like “It is something to do with arithmetics”, “It is something to do with nulls”. It’s to do with dereferencing so we can actually be much more concrete. 

### Underabstraction
There is a tecnique published in 2009:
Take your codebase and strip out your comments, strip out string literals, get rid of case sensitivity and put it into a tag cloud generator and see what you can see.

#### Example
![](https://github.com/readdeo91/clean-code-notes/blob/main/tagcloud2.png)
What we see first of all that the language is java. Beside import and public, we see that this system is a system of strings. That’s the dominant type. We also see list and integers and that’s kind of it. So what is this system about? There is no way to tell. This code is a failed attempt of object oriented programming.

![](https://github.com/readdeo91/clean-code-notes/blob/main/tagcloud.png)

This system is much richer: printingDevice, paper, product, testingPrinter. This is so much clearer.

```java
if (portfolioIdsByTraderId.get(trader.getid())
    .containsKey(portfolio.getId()))
```
The issue is to do with the lack of abstraction.

It’s not entirely clear of what it’s purpose. It’s just doesn’t communicate. It doesn’t fullfill the communication needs of it’s users.

```java
if (trader.canView(portfolio))
```

It’s much clearer. This is a permission check. It’s not just a lookup in a map of maps. It is **a technical expression of what is going on.** You can understand what it means. It means that whether or not this trader can view this portfolio.

