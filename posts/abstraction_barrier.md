# Software abstraction barrier

If we think about software as built in layers, we can imagine it as a pyramid where the lower levels are the closest to the hardware and the higher you go the closer you get to the user. Each level adds some abstraction to the one below it by hiding any implementation detail that is not useful to that particular level. This way, we can write code at a higher level while freeing our limited mental capacity by letting go of the need to know every implementation detail.

Thinking about abstraction also helps when working between teams as it helps with delegating responsibilities between teams.

For example, consider a scenario in which Team A is working on building a data pipeline for a machine learning project. Team B needs to access specific data in particular points as it's passing  through the data pipeline (e.g. for statistical analysis reasons). In this case we would have two possibilities:

1. Team A writes specific code every time Team B asks for something specific
2. Team A writes an abstraction over their existing code, e.g. a public function or library which Team B can directly use to write their specific tasks as needed.

By writing an abstraction over their existing code (read: layer in the software pyramid), Team A made it possible for Team B to access what they need without caring to learn how the underlying software layer is implemented. In other words, Team A implemented an abstraction barrier which is a layer of functions that hide the implementation so well that you can completely forget about how it is implemented even when using those functions. It should be noted that the "not caring" is symmetrical and works both ways. Team B does not care about how the data pipeline is implemented, but Team A also does not care about how Team B implement their solutions using the abstraction barrier.

## When to use abstraction barriers:

#### 1. To facilitate changes of implementation

In cases of high uncertainty about how you want to implement something, an abstraction barrier can be the layer of indirection that lets you change the implementation later. This property might be useful if you are prototyping something and you still don't know how best to implement it. Or perhaps you _know_ something will change; you're just not ready to do it yet, like if you know you will want to get data from the server eventually, but for now you'll just stub it out.

#### 2. To make code easier to write and read
Abstraction barriers allow us to ignore details. Sometimes those details are bug magnets. Did we initialize the loop variables correctly? Did we have an off-by-one error in the loop exit condition? An abstraction barrier that lets you ignore those details will make your code easier to write. If you hide the right details, then less adept programmers can be productive when using it.

#### 3. To reduce coordination between teams
Our development team could change things without talking with marketing first. And marketing could write simple marketing campaigns without checking in with development. The abstraction barrier allows teams on either side to ignore the details the other team handles. Thus, each team moves faster.

#### 4. To mentally focus on the problem at hand
They let you think more easily about the problem you are trying to solve. An abstraction barrier makes some details unimportant to the problem we are solving right now. It means we are less likely to make a mistake and less likely to get tired.