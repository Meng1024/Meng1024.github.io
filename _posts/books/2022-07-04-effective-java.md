---
layout:     post
title:      "Effective Java"
subtitle:   "Java"
categories:  Java
date:       2022-07-04
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - Java
---


# Summary

## Chapter 3 Methods Common To All Objects

### Item 10
The instanceof operator is specified to
return false if its first operand is null, regardless of what type appears in the
second operand [JLS, 15.20.2]. Therefore, the type check will return false if
null is passed in, so you don’t need an explicit null check.

## Chapter 4 Class and Interface

## Item 15
Instance fields of public classes should rarely be public (Item 16). If an
instance field is nonfinal or is a reference to a mutable object, then by making it
public, you give up the ability to limit the values that can be stored in the field.
This means you give up the ability to enforce invariants involving the field. Also,
you give up the ability to take any action when the field is modified, so classes
with public mutable fields are not generally thread-safe.

## Item 16
In summary, public classes should never expose mutable fields.

## Chapter 10  Exception

### Item 69/70
Use checked exceptions for conditions from which the caller
can reasonably be expected to recover. If a program throws an unchecked exception or an
error, it is generally the case that recovery is impossible and continued execution
would do more harm than good. If a program does not catch such a throwable, it
will cause the current thread to halt with an appropriate error message.

To summarize, throw checked exceptions for recoverable conditions and
unchecked exceptions for programming errors. When in doubt, throw unchecked
exceptions. Don’t define any throwables that are neither checked exceptions nor
runtime exceptions. Provide methods on your checked exceptions to aid in
recovery.

### Item 71
If a method throws
checked exceptions, the code that invokes it must handle them in one or more catch
blocks, or declare that it throws them and let them propagate outward. Either way, it
places a burden on the user of the API. The burden increased in Java 8, as methods
throwing checked exceptions can’t be used directly in streams

### Item 72
- Do not reuse Exception, RuntimeException, Throwable, or Error directly.
- Remember that exceptions are serializable (Chapter 12). That alone is reason not to
write your own exception class without good reason.

### Item 75
To capture a failure, the detail message of an exception should contain the
values of all parameters and fields that contributed to the exception.
do not include passwords, encryption keys, and the like in detail messages.

### Item 76
Generally speaking, a failed method invocation should leave
the object in the state that it was in prior to the invocation. A method with this
property is said to be failure-atomic.

the most common way to
achieve failure atomicity is to check parameters for validity before performing the
operation (Item 49).
