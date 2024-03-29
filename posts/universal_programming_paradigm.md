# Functional programming is a universal programming paradigm

Programming is essentially the activity of creating, transforming, and processing information in the form of data. The data itself is centerpiece to any programming activity and everything else around it serves to get the data where it needs to be.

Each piece of software can be thought of as a black box that communicates with the outside universe (to receive or send data). Within that black box we have operations on data which can be divided in two types: calculations and actions. 

Calculations are operations which, as the name suggests, calculate some value based on the data received as input and return an output on the other end. For the same input, we will always have the same output. For example, a banal calculation called `product` which finds the product of two integers, will always return the same results when given in input the same integers. 

Actions are operations for which the output either modifies the outside world or depends on the outside world. They differ from calculations because their output will not always be the same given the same input. For example, writing to a database is an action because the result of that write depends on factors other than the input (for example trying to insert a unique value twice).

Let’s take the example of a simple software that reads (action) a string from a file (data), extracts some substrings from it (calculation) and saves what it extracts to a database (action).

What we have defined with our observations here, is pretty much functional programming. Data is still data, calculations are pure functions, whereas actions are impure functions. But I strongly believe that such a paradigm applies to all programming, regardless or language or paradigm, so making the distinction between data, calculation, and action is an important skill to have as a programmer.