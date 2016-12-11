---
title: Defensive programming.
---
Quick overview of the `defensive programming` chapter from  Steve Mcconnell's book code complete. 

<!--more-->

The main concept about the defensive programming is that `don't let garbage out even if the routine takes garbage in`.
Always have a barricade between parts of code that work with `good data and bad data`. Usually public routines are considered to be working with bad data whereas 
private routines are considered to be dealing with more clean data.

The rationale between the barricade is that, all the code inside barricade should you `assertion` because data is supposed to be sterilized.
were as all the code outside barricade should use `error handling`. Some of the  error handling technique suggested here are **return a  neutral value**
in case the inputs are invalid, null etc. If your routine is called continously to process input and return value it is safe to return 
**the same answer as the previous time**. Other options would be **Log a warning message** or **return an error code**.

If the error handling cannot be done locally, and the condition is said to be exceptional only in such situations we should choose to pass an exception to 
the caller, The main catch here is that the exception thrown `should not break the level of abstraction` maintained in the codebase.
For example consider you have a routine named `getEmployeeTax()` this routine is responsible for the returning the employee tax, should not 
throw `EOF exception` in case of garbage input, as the exception exposes how employee is stored thereby breaking the encapsulation.Instead 
the routine can throw `EmployeeNotFoundException`.**Generally programmers should not use exceptions for error handling**. 
They should consider entire set of error handling first.

**Note:**
This post is just an overview of the chapter. 
Many nitty gritty details are omitted, so i personally recommend to get your copy of the book [today](https://www.amazon.com/Complete-Microsoft-Press-Steve-Mcconnell/dp/9350041243/ref=sr_1_1/255-2425890-2016030?ie=UTF8&qid=1481434800&sr=8-1&keywords=code+complete). 
