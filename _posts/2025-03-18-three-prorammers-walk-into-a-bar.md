three programmers walk into a bar.

the first, a staff engineers, says hey team I made a new method in the code.  it's a cool algo, and the method call takes about 10 microseconds.

the second, a senior staff engineer, says good work.  but we can improve this.  let's add
an interface and use spring to create the object 
using reflection.  this makes it more testable.
the performance cost is only 100 microseconds 
for the reflection, 10x slower.  but it is worth it.

the third, a principal enginner, says good work,
but we can make it better.  your solution lacks
developer velocity.  we can make a microservice
for this method.  the microservice takes 
10 milliseconds, 1,000,000x slower than the original method.  and we need to duplicate the great hygiene we invested in around the first project.  but you see now, if I need to change the validation logic that price is greater than zero, I can do it with greater velocity.

then a senior principal engineer arrives.
but he is late to the party, and the other three engineers have gone home.

he asks the bartender, have you seen the Enginners?  the bartender says yes, they were here, taking about microservices and developer velocity.  the senior principal says oh, I wish I had seen them, I would have told them to use modular monolith approach, oh well, can I have a drink pls, scotch on the rocks?

the bartender says, I gave up programming long ago, but back before it became complicated, we used to make big programs from software libraries, which we reused.  but you know what I say, the customer is always right, here is your scotch.



