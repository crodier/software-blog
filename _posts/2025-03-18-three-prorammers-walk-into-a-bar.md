Three Programmers walk into a bar.

The first, a Staff Engineer, says hey team I made 
a new method in the code.  It checks if the price
is valid; non-negative. The method 
call takes about 10 microseconds.

The second, a Senior Staff Engineer, says good work.  
But we can improve this.  let's add
an interface and use Spring to create the object 
using reflection.  this makes it more testable.
the performance cost is only 100 microseconds 
for the reflection, 10x slower.  but it is worth it.

The third, a Principal Engineer, says good work,
But we can make it better.  Your solution lacks
developer velocity.  We can make a microservice
for this method.  The microservice takes 
10 milliseconds, 1,000,000x slower than the original method.  
And we need to duplicate the great hygiene we invested 
in the first project.  
But you see now, if I need to change the validation 
logic that price is greater than zero, 
I can do it with greater velocity.

Then a senior principal engineer arrives.
But he is late to the party, and the other three 
engineers have gone home.

He asks the bartender, have you seen the Engineers?  
The bartender says yes, they were here, taking about 
microservices and developer velocity.  
The senior principal says oh, I wish I had seen them, 
I would have told them to use modular monolith approach, 
oh well, can I have a drink pls, scotch on the rocks?

The bartender says, I gave up programming long ago, 
but back before it became complicated, 
we used to make big programs from software libraries, 
which we reused.  But you know what I say, the customer 
is always right, here is your scotch.



