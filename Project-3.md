# Prolog and Declarative Programming Languages: Retraining Your Brain
By Grant Klich

Recently I worked on a social media post anaylzer using the Prolog logical programming language. Working with Prolog was like nothing I've every programmed in before, but I found my brain quickly adapting and developing analogies to imperative programming. I hope to explain declarative programming and share some Prolog tips in this article. 

## Declarative Programming. What's the Big Difference?

A quick Google search will give you the definition of declaritive versus imperative programming.

> **Declarative programming** is a programming paradigm … that expresses the logic of a computation without describing its control flow.

> **Imperative programming** is a programming paradigm that uses statements that change a program’s state

*Source: https://codeburst.io/declarative-vs-imperative-programming-a8a7c93d9ad2*

If you're like me, these definitions went right over my head alongside other fancy words like *paradigm* once did. And when I'm used to working in multi-paradigm languages like Scala (it's concurrent, functional, imperative, AND object-oriented according to Wikipedia), these paradigms all kind of blur together. Especially in multi-paradigm languages, I had trouble isolating exactly what was the declarative part. But let's see if I can successfully break down declarative programming with relatable examples before jumping in to the nightmare that is Prolog :).

### Example 1: HTML

While HTML is not a programming language (it is a markup language), it can be thought of as declarative anyway. When we create a `<div>`, we don't step-by-step tell the computer what to do to render this `<div>`. We're just declaring that it exists. It's up to browser engines to parse and procedurally tell the computer what to render. Note that there is no *state* in any part of HTML code. The lack of state is a big part of declarative programming.

Now let's cover an actual programming language. 

### Example 2: Functional Programming in JavaScript
 
Functional programming is subset of declarative programming that avoids state changes. My favorite clear example is comparing a for loop to a `.map` function in JavaScript

A for loop (imperative) to increment everything in the array by one:
```
for (var i = 0; i < arr.length; i++) {
	arr[i] = arr[i]+1;
}
```

We've seen for loops 1000 times. Notice we have to maintain the i variable, which is a state. This can easily lead to out of bounds errors and unintended consequences. 

The same algorithm with a `.map` call:
```
const newArray = array.map(function(e){
	return e+1;
});
```

This functional approach takes away the state by getting rid of `i`. There's virtually no room for error anymore, as the programmer simply *declares* what they want done. Map will not edit the original variable `array` so that we eliminate a confusing state change to `array`. We will know that `array` is always the old array and `newArray` is always the new array.


## "Converting" From a Imperative Frame of Mind in Prolog

Now that we know the elements of a declarative paradigm, let's look at the fully declarative language Prolog. Don't worry about syntax so much, as I'm merely explaining concepts here.

Prolog centers around declaring facts and making queries based on these facts.

So, for example, we could declare the facts:

```
friends(alice,bob).
friends(bob,charlie). 
```

By querying `friends(alice,X)`, Prolog will return all values of X that satisfy that fact (Here is would be `X = bob`).

### Imperative Analogies

Prolog doesn't have function, variables, call stacks, returns, or anything of that nature that we're all used to. This made figuring things out quite difficult at times.

Two facts I kept drilling in my head are as follows:
- If I must keep thinking of predicates as functions, remember that they only ever "return" yes or no
- If you want to "return a value", introduce a variable to the predicate.

These two facts kept me grounded every time I felt confused. In the second point, I kept building predicates that I wanted to return values. But that's obviously not the nature of Prolog. So what helped me the most was remembering that anytime I thought I needed to return a value in my predicate, it is essentially analogous to introducing a variable in Prolog. Then the "magic" really happens in the query. 

But that was a mouthful. Here's an dead simple comparison using the length -function- predicate, which -returns- represents the number of elements in a list:

In Python (a length function):
```
length = len([1,2,3])
```

In Prolog (a length predicate):
```
?- length([1,2,3], Length)

	Length=3 % This is a the result of the query
```

In Prolog, we put a variable (denoted by starting with a capital letter) in the query itself. So the intepretor looks for any value of Length that will make the length predicate be true. So we say that Prolog *unifies* Length with 3. If we don't use variables, querying `length([1,2,3],2)` would evaluate to `false`. Likewise, querying length([1,2,3],3) would evaluate to `true`.

Think of it kind of like the `scanf` function in C. Instead of saying `int myNumber = scanf("%d");`, we actually say `int myNum; scanf("%d", &myNum);`. The function "returns" the result into the provided parameter"

Now lets say we want to "return" the length of the string plus 3. Imperatively this is as easy as it sounds. But in Prolog it is a little trickier. We must introduce a new variable and constraint to the predicate.

In Python:
```
result =  len([1,2,3]) + 3
```

In Prolog:
```
?- length([1,2,3],Length), Result is Length+3. 

	Length=3,
	Result=6
```

In Prolog, we are saying we want the following query to be true: "We want a variable Length that equals the length of the array, and we want a variable Result that is Length+3. Hey interpreter, can you find these for me??" Thus, the interpreter unifies Length with 3 and Result with 6.

### A Conclusive Warning
I end this post with a warning that thinking about Prolog strictly in analogies to imperative programming is a bad idea in the long run. It's helpful to get you started, but it's important to realize the mechanics and purpose is completely different than imperative programming. Prolog is NOT simply a artifact of the past, replaced by imperative languages. 

As you play around with Prolog I think the change of mindset will naturally develop. It's like learning Spanish. First, you translate everything to English in your head to understand it, but eventually you start to eliminate the middleman and understand Spanish directly. But this crutch will help get you started much more easily.
