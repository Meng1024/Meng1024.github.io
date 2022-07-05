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

### item 71
If a method throws
checked exceptions, the code that invokes it must handle them in one or more catch
blocks, or declare that it throws them and let them propagate outward. Either way, it
places a burden on the user of the API. The burden increased in Java 8, as methods
throwing checked exceptions can’t be used directly in streams

### item 72
- Do not reuse Exception, RuntimeException, Throwable, or Error directly.
- Remember that exceptions are serializable (Chapter 12). That alone is reason not to
write your own exception class without good reason.

### item 75
To capture a failure, the detail message of an exception should contain the
values of all parameters and fields that contributed to the exception.
do not include passwords, encryption keys, and the like in detail messages.

### item 76
Generally speaking, a failed method invocation should leave
the object in the state that it was in prior to the invocation. A method with this
property is said to be failure-atomic.

the most common way to
achieve failure atomicity is to check parameters for validity before performing the
operation (Item 49).
