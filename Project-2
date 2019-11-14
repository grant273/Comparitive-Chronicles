# Comparative Chronicles: Project 2
By Grant Klich

## Scala: Is it *really* that concise? (A Comparison to Kotlin)

I recently worked on a project using Scala. One of the main selling points of Scala is its conciseness, mainly because of its emphasis on function programming. For example, a list can be filtered with the quite simple sytax of `myList.filter( _ > 5)`. (Wow! Simple!) The underscore is the implied parameter, so no need to set up a whole function header!

This conciseness works wonderfully for simple tasks. But I began to get confused writing longer statements and anything that required more than a very simple predicate. 


### Wait, What are Values Then?

In an [earlier project](https://grant273.github.io/Comparitive-Chronicles/) I highlighed the elegant syntax of functional parameters in Kotlin, such as `myArray.filter { it > 10 }`. The only functional parameter of a higher-order function can be given inside curly brackets after the function. This distinguishes functional-style predicates very well from parameter that are expressions. But in Scala, `myList.filter( _ > 5)`, aside from the underscore, there is no clear distinction between an value and a predicate.

I have not used Scala enough to really determine if that's a huge deal, but my initial thoughts are that it could cause confusion. For example, there is the case if you are filtering by some external condtion and have an unused predicate parameter: `myList.filter{someOtherVar == 5}` is valid in Kotlin, but `myList.filter(someOtherVar == 5)` is invalid in Scala. It must be expanded to `myList.filter((i) => someOtherVar == 5)` 

...Althought upon writing this, I realized this is a very stupid example because there's no reason to filter a list if you're not going to check if list element anyway. So maybe the presence of a `_` *is* just enough to distinguish a predicate from an expression. A function without a parameter is just a value. You win this round, Scala... 


### Multi-line Predicates

Round 2, let's try again.

It took me awhile to figure how to do a multiline functional parameter in Scala. In Kotlin, we can do:
```
myList.map{
	val t = it + 5
	t
} 
```

Notice the implied `it` parameter that we do not need to define.

In Scala, we have:
```
myList.map{ x =>
	val t = x + 5
	t
}
```

Notice the parenthesis changed to brackets (which I wonder why we don't just use in the first place, since `.map{ _ + 5}` is still legal). *And* notice that the parameter has to be explicitly definied this time (*groan*). So with Scala, the second we need more than one line for a functional parameter, the amount of effort goes WAY up. In Kotlin, it's as simple as adding line breaks.

### Kotlin's "it" vs Scala's "_"

Both `it` and `_` are rather mysterous keywords to programmers unfamiliar with either Kotlin or Scala, respectively. They generally function the same, as you can see in the previous example. They are placeholders for when it is unnecessary to define a parameter.

A big difference though, it that Scala's `_` will reference *different* variables sequentially if the function has multiple parameters. `it` is only defined and usable in 1-parameter functions.

The `reduce()` function is a great example of this behavior. It performs an operation on adjacent list elements until it collapses into a single value. For example `myList.reduce(_ + _)` would add each element together in Scala and calculate the sum of `myList`. Each mention of  `_` actually references to different elements. In Kotlin, however, we cannot be this concise. The equivalent Kotlin statement is `myList.reduce { a,b -> a + b }`. Bleh. We must define the left and right variable. This is minor, but gives Scala a decent edge for super quick multi-parameter predicates. 

At the same time, however, this makes Scala inconvenient for accessing the same parameter twice. Something like `.map(_*_)` to calculate a list of squares would not compile. We would need to do `.map(a => a*a)`

### Some or None? What?

In Scala, a common pattern is instead of functions sometimes returning null, a function will return an Option object, that is either a Some or None object. This forces the developer to handle null cases, and, if you're me, forces the developer to be annoyed and confused.

However, as I looked at my existing code, I realized it came very naturally to use Some/None instead of a boring old null check.

For example, I had something like:
```
if myList.exists(_.name == searchName) {
	val result = myList.find(_.name == searchName).get
	...
} else {
	...
}
```

This looked A LOT cleaner and efficient when I did:
```
myList.find(_.name == searchName) match {
	case Some(i) => ..
	case None => ...
}
```

As I've come to realize, null-checking is the bane of conciseness. Luckily, if you live dangerously, you can still use `.get` on an option, but the Option class provides for some really clean coding patterns.

### Conclusion

So as you can tell from this article alone, Scala starts as a complicatedly concise language, but the more one plays around and thinks about Scala, the more sense each thing makes, and the more one comes to appreciate it. I think the overly conciseness can make the language pretty confusing at first, but it's a delight once you get things all figured out.

Kotlin and Scala both take some getting used to. Both make assumptions that save you time for simple operations, but they also can make changing things tedious if your operation grows in requirements.
